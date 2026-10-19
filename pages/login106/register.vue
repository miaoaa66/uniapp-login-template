<template>
	<view class="mn-page">
		<view class="mn-status" :style="{ height: statusBar + 'px' }"></view>

		<view class="mn-nav">
			<view class="mn-back" @click="goBack">
				<view class="mn-back__line"></view>
				<view class="mn-back__tip"></view>
				<view class="mn-back__tip mn-back__tip--2"></view>
			</view>
			<text class="mn-nav__idx">02 / 03</text>
		</view>

		<view class="mn-main">
			<view class="mn-head">
				<text class="mn-eyebrow">ACCOUNT · SIGN UP</text>
				<text class="mn-title">创建账号</text>
				<text class="mn-lead">只需要一个手机号，其余资料可以稍后补充。</text>
			</view>

			<view class="mn-rule"></view>

			<view class="mn-form">
				<view class="mn-field">
					<view class="mn-field__bar">
						<text class="mn-label">手机号</text>
						<text class="mn-index">01</text>
					</view>
					<view class="mn-control" :class="{ 'is-focus': focus === 'phone', 'is-error': !!err.phone }">
						<input
							class="mn-input mn-input--num"
							v-model="phone"
							type="number"
							:placeholder-style="phStyle"
							placeholder="请输入 11 位手机号"
							maxlength="11"
							@focus="focus = 'phone'"
							@blur="focus = ''"
						/>
					</view>
					<text v-if="err.phone" class="mn-err">{{ err.phone }}</text>
				</view>

				<view class="mn-field">
					<view class="mn-field__bar">
						<text class="mn-label">短信验证码</text>
						<text class="mn-index">02</text>
					</view>
					<view class="mn-control" :class="{ 'is-focus': focus === 'sms', 'is-error': !!err.sms }">
						<input
							class="mn-input mn-input--num"
							v-model="sms"
							type="number"
							:placeholder-style="phStyle"
							placeholder="6 位数字"
							maxlength="6"
							@focus="focus = 'sms'"
							@blur="focus = ''"
						/>
						<text class="mn-code" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
					</view>
					<text v-if="err.sms" class="mn-err">{{ err.sms }}</text>
				</view>

				<view class="mn-field">
					<view class="mn-field__bar">
						<text class="mn-label">设置密码</text>
						<text class="mn-index">03</text>
					</view>
					<view class="mn-control" :class="{ 'is-focus': focus === 'password', 'is-error': !!err.password }">
						<input
							class="mn-input"
							v-model="password"
							:password="!showPwd"
							:placeholder-style="phStyle"
							placeholder="至少 8 位，含字母与数字"
							maxlength="20"
							@focus="focus = 'password'"
							@blur="focus = ''"
						/>
						<text class="mn-suffix" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
					</view>
					<text v-if="err.password" class="mn-err">{{ err.password }}</text>
					<text v-else class="mn-tip">建议使用字母、数字与符号的组合</text>
				</view>

				<view class="mn-field">
					<view class="mn-field__bar">
						<text class="mn-label">确认密码</text>
						<text class="mn-index">04</text>
					</view>
					<view class="mn-control" :class="{ 'is-focus': focus === 'confirm', 'is-error': !!err.confirm }">
						<input
							class="mn-input"
							v-model="confirm"
							:password="!showPwd"
							:placeholder-style="phStyle"
							placeholder="再次输入密码"
							maxlength="20"
							@focus="focus = 'confirm'"
							@blur="focus = ''"
						/>
					</view>
					<text v-if="err.confirm" class="mn-err">{{ err.confirm }}</text>
				</view>

				<view class="mn-btn" :class="{ 'is-busy': submitting }" @click="submit">注 册</view>

				<view class="mn-check" @click="agreed = !agreed">
					<view class="mn-check__box" :class="{ on: agreed }"><view class="mn-check__tick"></view></view>
					<text class="mn-check__txt">我已阅读并同意 <text class="mn-check__link">《用户协议》</text> 与 <text class="mn-check__link">《隐私政策》</text></text>
				</view>
			</view>
		</view>

		<view class="mn-foot">
			<view class="mn-foot__row">
				<text class="mn-foot__txt">已有账号</text>
				<text class="mn-link" @click="goLogin">返回登录</text>
			</view>
			<text class="mn-copy">© 2026 MINIMAL STUDIO · V1.0.0</text>
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
				phStyle: 'color:#8A8A8A;font-size:15px;',
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
				this.password = 'blank-2026';
				this.confirm = 'blank-2026';
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
				else uni.reLaunch({ url: '/pages/login106/login' });
			},

			goLogin() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/login106/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
