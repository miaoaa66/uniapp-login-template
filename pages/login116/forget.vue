<template>
	<view class="page">
		<view class="deco deco-stripe"></view>

		<view class="status" :style="{ height: statusBar + 'px' }"></view>

		<view class="nav">
			<view class="back" @click="goBack">
				<view class="back-line"></view>
				<view class="back-tip"></view>
				<view class="back-tip back-tip-2"></view>
			</view>
			<text class="nav-idx">03 / 03</text>
		</view>

		<view class="content">
			<view class="big">
				<text class="big-en">RESET</text>
				<view class="big-bar big-bar-orange"></view>
				<text class="big-cn">重置密码 / KEY LOST</text>
			</view>

			<view class="steps">
				<view class="step" :class="{ on: step === 0, done: step > 0 }">
					<text class="step-no">{{ step > 0 ? 'OK' : '01' }}</text>
					<text class="step-name">验证身份</text>
				</view>
				<view class="step" :class="{ on: step === 1 }">
					<text class="step-no">02</text>
					<text class="step-name">设置密码</text>
				</view>
			</view>

			<view class="forms">
				<block v-if="step === 0">
					<view class="bfield" :class="{ active: focus === 'phone', error: !!err.phone }">
						<text class="bfield-label">注册手机号 *</text>
						<input
							class="input input-num"
							v-model="phone"
							type="number"
							:placeholder-style="phStyle"
							placeholder="请输入 11 位手机号"
							maxlength="11"
							@focus="focus = 'phone'"
							@blur="focus = ''"
						/>
					</view>
					<text v-if="err.phone" class="err">{{ err.phone }}</text>

					<view class="bfield" :class="{ active: focus === 'sms', error: !!err.sms }">
						<text class="bfield-label">短信验证码 *</text>
						<input
							class="input input-num"
							v-model="sms"
							type="number"
							:placeholder-style="phStyle"
							placeholder="6 位数字"
							maxlength="6"
							@focus="focus = 'sms'"
							@blur="focus = ''"
						/>
						<text class="code" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
					</view>
					<text v-if="err.sms" class="err">{{ err.sms }}</text>

					<view class="giant giant-teal" @click="next">
						<text class="giant-txt">下一步 →</text>
					</view>
				</block>

				<block v-else>
					<view class="bfield" :class="{ active: focus === 'password', error: !!err.password }">
						<text class="bfield-label">新密码 *</text>
						<input
							class="input"
							v-model="password"
							:password="!showPwd"
							:placeholder-style="phStyle"
							placeholder="至少 8 位，含字母与数字"
							maxlength="20"
							@focus="focus = 'password'"
							@blur="focus = ''"
						/>
						<text class="eye" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
					</view>
					<text v-if="err.password" class="err">{{ err.password }}</text>

					<view class="bfield" :class="{ active: focus === 'confirm', error: !!err.confirm }">
						<text class="bfield-label">确认新密码 *</text>
						<input
							class="input"
							v-model="confirm"
							:password="!showPwd"
							:placeholder-style="phStyle"
							placeholder="再次输入新密码"
							maxlength="20"
							@focus="focus = 'confirm'"
							@blur="focus = ''"
						/>
					</view>
					<text v-if="err.confirm" class="err">{{ err.confirm }}</text>

					<view class="giant" :class="{ 'is-busy': submitting }" @click="submit">
						<text class="giant-txt">{{ submitting ? '处理中…' : '重置密码 →' }}</text>
					</view>
					<view class="back-btn" @click="back">← 上一步</view>
				</block>

				<view class="safe">
					<text class="safe-h">! 安全提示</text>
					<text class="safe-txt">重置成功后，其他设备上的登录状态将全部失效。若手机号已停用，请联系管理员核验身份。</text>
				</view>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text class="foot-txt">想起密码了？</text>
				<text class="link" @click="goLogin">返回登录 →</text>
			</view>
			<text class="copy">© 2026 BRUTAL SUPPLY CO.</text>
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
				phStyle: 'color:rgba(0,0,0,0.32);font-size:15px;',
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
					if (this.smsLeft > 0) this.smsLeft--;
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
				else uni.reLaunch({ url: '/pages/login116/login' });
			},

			goLogin() {
				uni.reLaunch({ url: '/pages/login116/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
