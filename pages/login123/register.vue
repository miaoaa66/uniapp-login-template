<template>
	<view class="yk-page">
		<view class="yk-bubble yk-bubble--1"></view>
		<view class="yk-bubble yk-bubble--2"></view>
		<view class="yk-bubble yk-bubble--3"></view>
		<view class="yk-spark yk-spark--1"></view>
		<view class="yk-spark yk-spark--2"></view>
		<view class="yk-spark yk-spark--3"></view>
		<text class="yk-side">Y2K ☆ NEW MEMBER ☆ 2000</text>

		<view class="yk-status" :style="{ height: statusBar + 'px' }"></view>

		<view class="yk-nav">
			<view class="yk-back" @click="goBack">
				<view class="yk-back__line"></view>
				<view class="yk-back__tip"></view>
				<view class="yk-back__tip yk-back__tip--2"></view>
			</view>
			<text class="yk-nav__idx">02 / 03</text>
		</view>

		<view class="yk-main">
			<view class="yk-card">
				<view class="yk-brand">
					<text class="yk-brand__dot"></text>
					<text class="yk-brand__txt">CHROME MAIL · 2000</text>
				</view>
				<text class="yk-title">新 成 员 登 记</text>

				<view class="yk-form">
					<view class="yk-field">
						<text class="yk-label"><text class="yk-label__dot"></text>PHONE · 手机号</text>
						<view class="yk-control" :class="{ 'is-focus': focus === 'phone', 'is-error': !!err.phone }">
							<input
								class="yk-input yk-input--num"
								v-model="phone"
								type="number"
								:placeholder-style="phStyle"
								placeholder="11 位手机号"
								maxlength="11"
								@focus="focus = 'phone'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.phone" class="yk-err">{{ err.phone }}</text>
					</view>

					<view class="yk-field">
						<text class="yk-label"><text class="yk-label__dot yk-label__dot--pink"></text>CODE · 短信验证码</text>
						<view class="yk-control" :class="{ 'is-focus': focus === 'sms', 'is-error': !!err.sms }">
							<input
								class="yk-input yk-input--num"
								v-model="sms"
								type="number"
								:placeholder-style="phStyle"
								placeholder="6 位数字"
								maxlength="6"
								@focus="focus = 'sms'"
								@blur="focus = ''"
							/>
							<text class="yk-suffix yk-suffix--code" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
						</view>
						<text v-if="err.sms" class="yk-err">{{ err.sms }}</text>
					</view>

					<view class="yk-field">
						<text class="yk-label"><text class="yk-label__dot"></text>PASSWORD · 设置密码</text>
						<view class="yk-control" :class="{ 'is-focus': focus === 'password', 'is-error': !!err.password }">
							<input
								class="yk-input"
								v-model="password"
								:password="!showPwd"
								:placeholder-style="phStyle"
								placeholder="至少 8 位，含字母与数字"
								maxlength="20"
								@focus="focus = 'password'"
								@blur="focus = ''"
							/>
							<text class="yk-suffix" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
						</view>
						<text v-if="err.password" class="yk-err">{{ err.password }}</text>
						<text v-else class="yk-tip">密码就是你小灵通的解锁码升级版</text>
					</view>

					<view class="yk-field">
						<text class="yk-label"><text class="yk-label__dot yk-label__dot--pink"></text>CHECK · 确认密码</text>
						<view class="yk-control" :class="{ 'is-focus': focus === 'confirm', 'is-error': !!err.confirm }">
							<input
								class="yk-input"
								v-model="confirm"
								:password="!showPwd"
								:placeholder-style="phStyle"
								placeholder="再次输入密码"
								maxlength="20"
								@focus="focus = 'confirm'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.confirm" class="yk-err">{{ err.confirm }}</text>
					</view>

					<view class="yk-btn" :class="{ 'is-busy': submitting }" @click="submit">入 网 登 记</view>

					<view class="yk-check" @click="agreed = !agreed">
						<view class="yk-check__box" :class="{ on: agreed }"><view class="yk-check__tick"></view></view>
						<text class="yk-check__txt">我已阅读并同意 <text class="yk-check__link">《用户协议》</text> 与 <text class="yk-check__link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
		</view>

		<view class="yk-foot">
			<view class="yk-foot__row">
				<text class="yk-foot__txt">已经是老网民了</text>
				<text class="yk-link" @click="goLogin">回去连线 →</text>
			</view>
			<text class="yk-copy">© 2000-2026 CHROME MAIL · DIAL-UP FOREVER</text>
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
				phStyle: 'color:#8FA2B2;font-size:15px;',
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
				this.password = 'chrome-2000';
				this.confirm = 'chrome-2000';
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
				else uni.reLaunch({ url: '/pages/login123/login' });
			},

			goLogin() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/login123/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
