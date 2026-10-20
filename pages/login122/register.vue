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
				<text class="og-nav__idx">02 / 03</text>
			</view>

			<view class="og-crown__body">
				<text class="og-eyebrow">NEW SEED · 新芽登记</text>
				<text class="og-title">种下一颗种子</text>
				<text class="og-lead">只需要一个手机号，其余资料可以以后慢慢生长。</text>
			</view>
		</view>

		<view class="og-main">
			<view class="og-card og-card--push">
				<view class="og-form">
					<view class="og-field" :class="{ 'is-float': focus === 'phone' || phone, 'is-error': !!err.phone }">
						<text class="og-label">手机号</text>
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

					<view class="og-field" :class="{ 'is-float': focus === 'password' || password, 'is-error': !!err.password }">
						<text class="og-label">设置密码</text>
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
						<text v-else class="og-tip">建议字母、数字与符号的组合</text>
					</view>

					<view class="og-field" :class="{ 'is-float': focus === 'confirm' || confirm, 'is-error': !!err.confirm }">
						<text class="og-label">确认密码</text>
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

					<view class="og-btn" :class="{ 'is-busy': submitting }" @click="submit">让 种 子 发 芽</view>

					<view class="og-check" @click="agreed = !agreed">
						<view class="og-check__box" :class="{ on: agreed }"><view class="og-check__tick"></view></view>
						<text class="og-check__txt">我已阅读并同意 <text class="og-check__link">《用户协议》</text> 与 <text class="og-check__link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
		</view>

		<view class="og-foot">
			<view class="og-foot__hill">
				<view class="og-foot__row">
					<text class="og-foot__txt">已经是一棵树了</text>
					<text class="og-link" @click="goLogin">回到绿洲 →</text>
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
				focus: '',
				showPwd: false,
				agreed: false,
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
			if (options && options.demo) this.fillDemo();
		},
		onUnload() {
			this.clearTimer();
		},
		methods: {
			fillDemo() {
				this.agreed = true;
				this.phone = '13800001234';
				this.sms = '202610';
				this.password = 'organic-7211';
				this.confirm = 'organic-7211';
			},

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

			validate() {
				const e = {};
				if (!this.isPhone(this.phone)) e.phone = '请输入正确的手机号';
				if (!this.sms) e.sms = '请输入短信验证码';
				if (!this.password) e.password = '请设置登录密码';
				else if (this.password.length < 8) e.password = '密码不少于 8 位';
				else if (!/[a-zA-Z]/.test(this.password) || !/\d/.test(this.password)) e.password = '密码需同时包含字母与数字';
				if (!this.confirm) e.confirm = '请再次输入密码';
				else if (this.confirm !== this.password) e.confirm = '两次输入的密码不一致';
				this.err = e;
				return Object.keys(e).length === 0;
			},

			submit() {
				if (this.submitting) return;
				if (!this.agreed) return this.toast('请先阅读并同意用户协议');
				if (!this.validate()) return;
				this.submitting = true;
				// TODO: 调用后端注册接口，成功后自动登录
				setTimeout(() => {
					this.submitting = false;
					this.toast('账号创建成功');
				}, 600);
			},

			goBack() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/login122/login' });
			},

			goLogin() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/login122/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
