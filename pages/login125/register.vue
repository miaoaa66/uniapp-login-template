<template>
	<view class="in-page">
		<view class="in-status" :style="{ height: statusBar + 'px' }"></view>

		<view class="in-nav">
			<view class="in-back" @click="goBack">
				<view class="in-back__line"></view>
				<view class="in-back__tip"></view>
				<view class="in-back__tip in-back__tip--2"></view>
			</view>
			<text class="in-nav__idx">02 / 03</text>
		</view>

		<view class="in-plate">
			<view class="in-rivet in-rivet--tl"></view>
			<view class="in-rivet in-rivet--tr"></view>
			<view class="in-rivet in-rivet--bl"></view>
			<view class="in-rivet in-rivet--br"></view>
			<text class="in-plate__code">NEW OPERATOR · REG-08</text>
			<text class="in-plate__title">操 作 员 注 册</text>
			<text class="in-plate__sub">录入终端号并设定访问密钥，完成后自动签发上岗证。</text>
		</view>
		<view class="in-hazard in-hazard--thin"></view>

		<view class="in-cabinet">
			<view class="in-lamps">
				<view class="in-lamp">
					<view class="in-lamp__bulb in-lamp__bulb--pwr on"></view>
					<text class="in-lamp__txt">PWR</text>
				</view>
				<view class="in-lamp">
					<view class="in-lamp__bulb in-lamp__bulb--act" :class="{ on: submitting }"></view>
					<text class="in-lamp__txt">ACT</text>
				</view>
				<view class="in-lamp">
					<view class="in-lamp__bulb in-lamp__bulb--err" :class="{ on: hasErr() }"></view>
					<text class="in-lamp__txt">ERR</text>
				</view>
			</view>

			<view class="in-form">
				<view class="in-field">
					<view class="in-field__bar">
						<text class="in-label">TERMINAL / 手机号</text>
						<text class="in-code">REG-01</text>
					</view>
					<view class="in-slot" :class="{ 'is-focus': focus === 'phone', 'is-error': !!err.phone }">
						<input
							class="in-input in-input--num"
							v-model="phone"
							type="number"
							:placeholder-style="phStyle"
							placeholder="11 位手机号"
							maxlength="11"
							@focus="focus = 'phone'"
							@blur="focus = ''"
						/>
					</view>
					<text v-if="err.phone" class="in-err">{{ err.phone }}</text>
				</view>

				<view class="in-field">
					<view class="in-field__bar">
						<text class="in-label">RELAY CODE / 短信验证码</text>
						<text class="in-code">REG-02</text>
					</view>
					<view class="in-slot" :class="{ 'is-focus': focus === 'sms', 'is-error': !!err.sms }">
						<input
							class="in-input in-input--num"
							v-model="sms"
							type="number"
							:placeholder-style="phStyle"
							placeholder="6 位数字"
							maxlength="6"
							@focus="focus = 'sms'"
							@blur="focus = ''"
						/>
						<text class="in-suffix in-suffix--code" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
					</view>
					<text v-if="err.sms" class="in-err">{{ err.sms }}</text>
				</view>

				<view class="in-field">
					<view class="in-field__bar">
						<text class="in-label">SET KEY / 设置密码</text>
						<text class="in-code">REG-03</text>
					</view>
					<view class="in-slot" :class="{ 'is-focus': focus === 'password', 'is-error': !!err.password }">
						<input
							class="in-input"
							v-model="password"
							:password="!showPwd"
							:placeholder-style="phStyle"
							placeholder="至少 8 位，含字母与数字"
							maxlength="20"
							@focus="focus = 'password'"
							@blur="focus = ''"
						/>
						<text class="in-suffix" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
					</view>
					<text v-if="err.password" class="in-err">{{ err.password }}</text>
					<text v-else class="in-tip">字母 + 数字，扭矩等级不低于 8 位</text>
				</view>

				<view class="in-field">
					<view class="in-field__bar">
						<text class="in-label">CONFIRM / 确认密码</text>
						<text class="in-code">REG-04</text>
					</view>
					<view class="in-slot" :class="{ 'is-focus': focus === 'confirm', 'is-error': !!err.confirm }">
						<input
							class="in-input"
							v-model="confirm"
							:password="!showPwd"
							:placeholder-style="phStyle"
							placeholder="再次输入密码"
							maxlength="20"
							@focus="focus = 'confirm'"
							@blur="focus = ''"
						/>
					</view>
					<text v-if="err.confirm" class="in-err">{{ err.confirm }}</text>
				</view>

				<view class="in-eStop" :class="{ 'is-busy': submitting }" @click="submit">提 交 · 备 案</view>

				<view class="in-check" @click="agreed = !agreed">
					<view class="in-check__box" :class="{ on: agreed }"><view class="in-check__tick"></view></view>
					<text class="in-check__txt">我已阅读并同意 <text class="in-check__link">《用户协议》</text> 与 <text class="in-check__link">《隐私政策》</text></text>
				</view>

				<view class="in-ruler"></view>
			</view>
		</view>

		<view class="in-hazard in-hazard--band"></view>

		<view class="in-foot">
			<view class="in-foot__row">
				<text class="in-foot__txt">已是持证操作员</text>
				<text class="in-link" @click="goLogin">返回控制台 →</text>
			</view>
			<text class="in-copy">© 2026 HEAVY DUTY ACCESS · SPEC-07 REV.C</text>
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
				phStyle: 'color:#5E646B;font-size:15px;',
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
				this.password = 'torque-7708';
				this.confirm = 'torque-7708';
			},

			hasErr() {
				return Object.keys(this.err).length > 0;
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
				else uni.reLaunch({ url: '/pages/login125/login' });
			},

			goLogin() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/login125/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
