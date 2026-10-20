<template>
	<view class="og-page">
		<view class="og-crown">
			<view class="og-crown__leaf og-crown__leaf--l"></view>
			<view class="og-crown__leaf og-crown__leaf--r"></view>
			<view class="og-status" :style="{ height: statusBar + 'px' }"></view>

			<view class="og-nav">
				<view class="og-back" @click="goBack">
					<view class="og-back__line"></view>
					<view class="og-back__tip"></view>
					<view class="og-back__tip og-back__tip--2"></view>
				</view>
				<text class="og-nav__idx">03 / 03</text>
			</view>

			<view class="og-crown__body">
				<text class="og-eyebrow">RE-GROW · 重新生长</text>
				<text class="og-title">找回密码</text>
				<text class="og-lead">验证身份，然后为账号重新长出一把新钥匙。</text>
			</view>
		</view>

		<view class="og-main">
			<view class="og-card og-card--push">
				<!-- 步骤：藤蔓上的两颗种子 -->
				<view class="og-vine">
					<view class="og-vine__seed" :class="{ on: step >= 0, done: step > 0 }">
						<view class="og-vine__dot"></view>
						<text class="og-vine__txt">01 验证身份</text>
					</view>
					<view class="og-vine__stem" :class="{ grow: step > 0 }"></view>
					<view class="og-vine__seed" :class="{ on: step > 0 }">
						<view class="og-vine__dot"></view>
						<text class="og-vine__txt">02 设置密码</text>
					</view>
				</view>

				<view class="og-form">
					<!-- 第一步：验证身份 -->
					<block v-if="step === 0">
						<view class="og-field" :class="{ 'is-float': focus === 'phone' || phone, 'is-error': !!err.phone }">
							<text class="og-label">注册手机号</text>
							<input
								class="og-input og-input--num"
								v-model="phone"
								type="number"
								:placeholder-style="phStyle"
								placeholder=" "
								maxlength="11"
								@focus="focus = 'phone'"
								@blur="focus = ''"
							/>
							<text v-if="err.phone" class="og-err">{{ err.phone }}</text>
						</view>

						<view class="og-field" :class="{ 'is-float': focus === 'sms' || sms, 'is-error': !!err.sms }">
							<text class="og-label">短信验证码</text>
							<view class="og-inputrow">
								<input
									class="og-input og-input--num"
									v-model="sms"
									type="number"
									:placeholder-style="phStyle"
									placeholder=" "
									maxlength="6"
									@focus="focus = 'sms'"
									@blur="focus = ''"
								/>
								<text class="og-suffix og-suffix--code" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
							</view>
							<text v-if="err.sms" class="og-err">{{ err.sms }}</text>
						</view>

						<view class="og-btn" @click="next">下 一 步</view>
					</block>

					<!-- 第二步：设置新密码 -->
					<block v-else>
						<view class="og-field" :class="{ 'is-float': focus === 'password' || password, 'is-error': !!err.password }">
							<text class="og-label">新密码</text>
							<view class="og-inputrow">
								<input
									class="og-input"
									v-model="password"
									:password="!showPwd"
									:placeholder-style="phStyle"
									placeholder=" "
									maxlength="20"
									@focus="focus = 'password'"
									@blur="focus = ''"
								/>
								<text class="og-suffix" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
							</view>
							<text v-if="err.password" class="og-err">{{ err.password }}</text>
						</view>

						<view class="og-field" :class="{ 'is-float': focus === 'confirm' || confirm, 'is-error': !!err.confirm }">
							<text class="og-label">确认新密码</text>
							<view class="og-inputrow">
								<input
									class="og-input"
									v-model="confirm"
									:password="!showPwd"
									:placeholder-style="phStyle"
									placeholder=" "
									maxlength="20"
									@focus="focus = 'confirm'"
									@blur="focus = ''"
								/>
							</view>
							<text v-if="err.confirm" class="og-err">{{ err.confirm }}</text>
						</view>

						<view class="og-btn" :class="{ 'is-busy': submitting }" @click="submit">重 置 密 码</view>
						<view class="og-btn og-btn--ghost" @click="back">上一步</view>
					</block>

					<view class="og-note">
						<text class="og-note__h">安全提示</text>
						<text class="og-note__txt">重置成功后，其他设备上的登录状态将全部失效。若手机号已停用，请联系管理员核验身份。</text>
					</view>
				</view>
			</view>
		</view>

		<view class="og-foot">
			<view class="og-foot__hill">
				<view class="og-foot__row">
					<text class="og-foot__txt">想起密码了</text>
					<text class="og-link" @click="goLogin">返回登录 →</text>
				</view>
				<text class="og-copy">© 2026 ORGANIC GROVE · 慢慢生长</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				statusBar: 0,
				step: 0,
				focus: '',
				showPwd: false,
				submitting: false,
				phStyle: 'color:#9AA88F;font-size:15px;',
				phone: '',
				sms: '',
				password: '',
				confirm: '',
				smsLeft: 0,
				timer: null,
				err: {}
			};
		},
		onLoad(options) {
			const info = uni.getSystemInfoSync();
			this.statusBar = info.statusBarHeight || 0;
			if (options && options.demo) {
				this.phone = '13800001234';
				this.sms = '202610';
			}
		},
		onUnload() {
			this.clearTimer();
		},
		methods: {
			toast(title) {
				uni.showToast({ title, icon: 'none' });
			},

			isPhone(v) {
				return /^1\d{10}$/.test(v);
			},

			sendCode() {
				if (this.smsLeft > 0) return;
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号');
				this.smsLeft = 60;
				// TODO: 接入后端验证码接口
				this.toast('验证码已发送');
				if (this.timer) return;
				this.timer = setInterval(() => {
					this.smsLeft--;
					if (this.smsLeft <= 0) this.clearTimer();
				}, 1000);
			},

			clearTimer() {
				if (this.timer) {
					clearInterval(this.timer);
					this.timer = null;
				}
			},

			next() {
				const e = {};
				if (!this.isPhone(this.phone)) e.phone = '请输入正确的手机号';
				if (!this.sms) e.sms = '请输入短信验证码';
				else if (this.sms.length !== 6) e.sms = '验证码为 6 位数字';
				this.err = e;
				if (Object.keys(e).length) return;
				// TODO: 校验验证码是否通过
				this.clearTimer();
				this.smsLeft = 0;
				this.err = {};
				this.step = 1;
			},

			back() {
				this.err = {};
				this.step = 0;
			},

			validate() {
				const e = {};
				if (!this.password) e.password = '请设置新密码';
				else if (this.password.length < 8) e.password = '密码不少于 8 位';
				else if (!/[a-zA-Z]/.test(this.password) || !/\d/.test(this.password)) e.password = '密码需同时包含字母与数字';
				if (!this.confirm) e.confirm = '请再次输入新密码';
				else if (this.confirm !== this.password) e.confirm = '两次输入的密码不一致';
				this.err = e;
				return Object.keys(e).length === 0;
			},

			submit() {
				if (this.submitting) return;
				if (!this.validate()) return;
				this.submitting = true;
				// TODO: 调用后端重置密码接口
				setTimeout(() => {
					this.submitting = false;
					this.toast('密码已重置');
				}, 600);
			},

			goBack() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/login122/login' });
			},

			goLogin() {
				uni.reLaunch({ url: '/pages/login122/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
