<template>
	<view class="sw-page">
		<view class="sw-grid"></view>
		<view class="sw-status" :style="{ height: statusBar + 'px' }"></view>

		<view class="sw-top">
			<view class="sw-back" @click="goBack">
				<view class="sw-back__arrow"></view>
			</view>
			<text class="sw-top__meta">SWISS GRID SYSTEM · 12 COL / CH</text>
			<text class="sw-top__idx">03—126</text>
		</view>
		<view class="sw-rule"></view>

		<view class="sw-hero">
			<view class="sw-hero__accent"></view>
			<view class="sw-hero__head">
				<text class="sw-hero__num">03</text>
				<view class="sw-hero__meta">
					<text class="sw-hero__kicker">NEW ACCOUNT</text>
					<text class="sw-hero__kicker sw-hero__kicker--dim">创建账号 · REGISTRIEREN</text>
				</view>
			</view>
			<text class="sw-hero__title">注册</text>
			<text class="sw-hero__en">REGISTER / KONTO ANLEGEN</text>
			<text class="sw-hero__desc">四项信息，一条基线。所有字段按同一网格左对齐排列。</text>
		</view>

		<view class="sw-form">
			<view
				class="sw-field"
				:class="{ 'is-focus': focus === 'phone', 'is-on': focus === 'phone' || !!phone, 'is-error': !!err.phone }"
			>
				<text class="sw-field__label">手机号<text class="sw-field__req">*</text> / MOBILE</text>
				<text class="sw-field__idx">01</text>
				<input
					class="sw-field__input"
					v-model="phone"
					type="number"
					:placeholder-style="phStyle"
					placeholder=""
					maxlength="11"
					@focus="focus = 'phone'"
					@blur="focus = ''"
				/>
				<view class="sw-field__line"><view class="sw-field__ink"></view></view>
			</view>
			<text v-if="err.phone" class="sw-err">{{ err.phone }}</text>

			<view
				class="sw-field sw-field--code"
				:class="{ 'is-focus': focus === 'sms', 'is-on': focus === 'sms' || !!sms, 'is-error': !!err.sms }"
			>
				<text class="sw-field__label">验证码<text class="sw-field__req">*</text> / SMS CODE</text>
				<text class="sw-field__idx">02</text>
				<input
					class="sw-field__input"
					v-model="sms"
					type="number"
					:placeholder-style="phStyle"
					placeholder=""
					maxlength="6"
					@focus="focus = 'sms'"
					@blur="focus = ''"
				/>
				<text class="sw-field__code" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
				<view class="sw-field__line"><view class="sw-field__ink"></view></view>
			</view>
			<text v-if="err.sms" class="sw-err">{{ err.sms }}</text>

			<view
				class="sw-field sw-field--toggle"
				:class="{ 'is-focus': focus === 'password', 'is-on': focus === 'password' || !!password, 'is-error': !!err.password }"
			>
				<text class="sw-field__label">设置密码<text class="sw-field__req">*</text> / PASSWORD</text>
				<text class="sw-field__idx">03</text>
				<input
					class="sw-field__input"
					v-model="password"
					:password="!showPwd"
					:placeholder-style="phStyle"
					placeholder=""
					maxlength="20"
					@focus="focus = 'password'"
					@blur="focus = ''"
				/>
				<text class="sw-field__toggle" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
				<view class="sw-field__line"><view class="sw-field__ink"></view></view>
			</view>
			<view v-if="password" class="sw-strength">
				<view class="sw-strength__bars">
					<view v-for="n in 4" :key="n" class="sw-strength__bar" :class="{ on: n <= strength }"></view>
				</view>
				<text class="sw-strength__txt">{{ strengthText }}</text>
			</view>
			<text v-if="err.password" class="sw-err">{{ err.password }}</text>
			<text v-else class="sw-hint">至少 8 位，同时包含字母与数字</text>

			<view
				class="sw-field sw-field--toggle"
				:class="{ 'is-focus': focus === 'confirm', 'is-on': focus === 'confirm' || !!confirm, 'is-error': !!err.confirm }"
			>
				<text class="sw-field__label">确认密码<text class="sw-field__req">*</text> / CONFIRM</text>
				<text class="sw-field__idx">04</text>
				<input
					class="sw-field__input"
					v-model="confirm"
					:password="!showPwd"
					:placeholder-style="phStyle"
					placeholder=""
					maxlength="20"
					@focus="focus = 'confirm'"
					@blur="focus = ''"
				/>
				<view class="sw-field__line"><view class="sw-field__ink"></view></view>
			</view>
			<text v-if="err.confirm" class="sw-err">{{ err.confirm }}</text>

			<view class="sw-submit" :class="{ 'is-busy': submitting }" @click="submit">
				<text class="sw-submit__txt">创 建 账 号</text>
				<view class="sw-submit__right">
					<text class="sw-submit__en">CREATE</text>
					<text class="sw-submit__arrow">→</text>
				</view>
			</view>

			<view class="sw-agree" @click="agreed = !agreed">
				<view class="sw-check__box" :class="{ on: agreed }"><view class="sw-check__tick"></view></view>
				<text class="sw-agree__txt">我已阅读并同意 <text class="sw-agree__link">《用户协议》</text> 与 <text class="sw-agree__link">《隐私政策》</text></text>
			</view>
		</view>

		<view class="sw-foot">
			<view class="sw-foot__rule"></view>
			<view class="sw-foot__body">
				<text class="sw-foot__txt">已有账号？<text class="sw-link" @click="goLogin">返回登录</text></text>
				<text class="sw-foot__meta">SWISS · 02/20</text>
			</view>
			<view class="sw-foot__bar">
				<text>© 2026 GRID SYSTEM</text>
				<text>750 / 12 COL</text>
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
				phStyle: 'color:#A7A39B;font-size:30rpx;',
				phone: '',
				sms: '',
				password: '',
				confirm: '',
				smsLeft: 0,
				timer: null,
				err: {}
			};
		},
		computed: {
			strength() {
				const v = this.password;
				let s = 0;
				if (v.length >= 8) s++;
				if (/[a-zA-Z]/.test(v)) s++;
				if (/\d/.test(v)) s++;
				if (/[^a-zA-Z0-9]/.test(v)) s++;
				return s;
			},
			strengthText() {
				return ['—', '弱', '一般', '较强', '强'][this.strength];
			}
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
				this.sms = '126126';
				this.password = 'helvetica-1957';
				this.confirm = 'helvetica-1957';
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
				else uni.reLaunch({ url: '/pages/login126/login' });
			},

			goLogin() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/login126/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
