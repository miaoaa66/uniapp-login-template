<template>
	<view class="vp-page">
		<view class="vp-sky">
			<view class="vp-sun"></view>
			<view class="vp-star vp-star--1"></view>
			<view class="vp-star vp-star--2"></view>
			<view class="vp-star vp-star--3"></view>
			<text class="vp-kana">ヴェイパーウェイヴ · 新規登録</text>
		</view>
		<view class="vp-horizon"></view>
		<view class="vp-grid">
			<view class="vp-grid__mesh"></view>
		</view>
		<view class="vp-scan"></view>

		<view class="vp-status" :style="{ height: statusBar + 'px' }"></view>

		<view class="vp-nav">
			<view class="vp-back" @click="goBack">
				<view class="vp-back__line"></view>
				<view class="vp-back__tip"></view>
				<view class="vp-back__tip vp-back__tip--2"></view>
			</view>
			<text class="vp-nav__idx">02 / 03</text>
		</view>

		<view class="vp-panel">
			<view class="vp-head">
				<text class="vp-eyebrow">NEW LISTENER · 新听众登记</text>
				<text class="vp-title">领一张入场券</text>
				<text class="vp-lead">留下手机号，电台会把梦境入场券寄给你。</text>
			</view>

			<view class="vp-form">
				<view class="vp-field">
					<text class="vp-label">PHONE / 手机号</text>
					<view class="vp-control" :class="{ 'is-focus': focus === 'phone', 'is-error': !!err.phone }">
						<input
							class="vp-input vp-input--num"
							v-model="phone"
							type="number"
							:placeholder-style="phStyle"
							placeholder="11 位手机号"
							maxlength="11"
							@focus="focus = 'phone'"
							@blur="focus = ''"
						/>
					</view>
					<text v-if="err.phone" class="vp-err">{{ err.phone }}</text>
				</view>

				<view class="vp-field">
					<text class="vp-label">CODE / 短信验证码</text>
					<view class="vp-control" :class="{ 'is-focus': focus === 'sms', 'is-error': !!err.sms }">
						<input
							class="vp-input vp-input--num"
							v-model="sms"
							type="number"
							:placeholder-style="phStyle"
							placeholder="6 位数字"
							maxlength="6"
							@focus="focus = 'sms'"
							@blur="focus = ''"
						/>
						<text class="vp-suffix vp-suffix--code" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
					</view>
					<text v-if="err.sms" class="vp-err">{{ err.sms }}</text>
				</view>

				<view class="vp-field">
					<text class="vp-label">PASSWORD / 设置密码</text>
					<view class="vp-control" :class="{ 'is-focus': focus === 'password', 'is-error': !!err.password }">
						<input
							class="vp-input"
							v-model="password"
							:password="!showPwd"
							:placeholder-style="phStyle"
							placeholder="至少 8 位，含字母与数字"
							maxlength="20"
							@focus="focus = 'password'"
							@blur="focus = ''"
						/>
						<text class="vp-suffix" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
					</view>
					<text v-if="err.password" class="vp-err">{{ err.password }}</text>
					<text v-else class="vp-tip">建议字母、数字与符号的组合</text>
				</view>

				<view class="vp-field">
					<text class="vp-label">CHECK / 确认密码</text>
					<view class="vp-control" :class="{ 'is-focus': focus === 'confirm', 'is-error': !!err.confirm }">
						<input
							class="vp-input"
							v-model="confirm"
							:password="!showPwd"
							:placeholder-style="phStyle"
							placeholder="再次输入密码"
							maxlength="20"
							@focus="focus = 'confirm'"
							@blur="focus = ''"
						/>
					</view>
					<text v-if="err.confirm" class="vp-err">{{ err.confirm }}</text>
				</view>

				<view class="vp-btn" :class="{ 'is-busy': submitting }" @click="submit">注 册 听 众</view>

				<view class="vp-check" @click="agreed = !agreed">
					<view class="vp-check__box" :class="{ on: agreed }"><view class="vp-check__tick"></view></view>
					<text class="vp-check__txt">我已阅读并同意 <text class="vp-check__link">《用户协议》</text> 与 <text class="vp-check__link">《隐私政策》</text></text>
				</view>
			</view>
		</view>

		<view class="vp-foot">
			<view class="vp-foot__row">
				<text class="vp-foot__txt">已经有入场券了</text>
				<text class="vp-link" @click="goLogin">去登录 →</text>
			</view>
			<text class="vp-copy">© 1989-2026 VAPORWAVE FM · SLOW DREAM</text>
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
				phStyle: 'color:#6E64A0;font-size:15px;',
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
				this.password = 'neon-1989';
				this.confirm = 'neon-1989';
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
				else uni.reLaunch({ url: '/pages/login124/login' });
			},

			goLogin() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/login124/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
