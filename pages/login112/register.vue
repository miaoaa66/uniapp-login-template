<template>
	<view class="page">
		<view class="deco deco-grid"></view>
		<view class="deco deco-glow deco-glow-a"></view>
		<view class="deco deco-glow deco-glow-b"></view>
		<view class="deco deco-bracket deco-bracket-tl"></view>
		<view class="deco deco-bracket deco-bracket-br"></view>
		<view class="deco deco-scan"></view>

		<view class="status" :style="{ height: statusBar + 'px' }"></view>

		<view class="nav">
			<view class="back" @click="goBack">
				<view class="back-line"></view>
				<view class="back-tip"></view>
				<view class="back-tip back-tip-2"></view>
			</view>
			<text class="nav-idx">02 / 03</text>
		</view>

		<view class="content">
			<view class="brand">
				<view class="logo"><view class="logo-core"></view></view>
				<view class="brand-txt">
					<text class="brand-name">霓虹终端</text>
					<text class="brand-sub">NIGHT CITY // NODE 07</text>
				</view>
			</view>

			<view class="head">
				<text class="eyebrow">REGISTER // NODE</text>
				<text class="title">创建身份</text>
				<text class="lead">注册一个新的接入身份，绑定手机号以接收校验码。</text>
			</view>

			<view class="card">
				<view class="field">
					<text class="field-label">手机号</text>
					<view class="field-box" :class="{ active: focus === 'phone', error: !!err.phone }">
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
				</view>

				<view class="field">
					<text class="field-label">短信验证码</text>
					<view class="field-box" :class="{ active: focus === 'sms', error: !!err.sms }">
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
						<text class="code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
					</view>
					<text v-if="err.sms" class="err">{{ err.sms }}</text>
				</view>

				<view class="field">
					<text class="field-label">设置密码</text>
					<view class="field-box" :class="{ active: focus === 'password', error: !!err.password }">
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
					<text v-else class="tip">建议使用字母、数字与符号的组合</text>
				</view>

				<view class="field">
					<text class="field-label">确认密码</text>
					<view class="field-box" :class="{ active: focus === 'confirm', error: !!err.confirm }">
						<input
							class="input"
							v-model="confirm"
							:password="!showPwd"
							:placeholder-style="phStyle"
							placeholder="再次输入密码"
							maxlength="20"
							@focus="focus = 'confirm'"
							@blur="focus = ''"
						/>
					</view>
					<text v-if="err.confirm" class="err">{{ err.confirm }}</text>
				</view>

				<view class="btn" :class="{ 'is-busy': submitting }" @click="submit">注 册</view>

				<view class="check" @click="agreed = !agreed">
					<view class="check-box" :class="{ on: agreed }"><view class="check-tick"></view></view>
					<text class="check-txt">我已阅读并同意 <text class="check-link">《用户协议》</text> 与 <text class="check-link">《隐私政策》</text></text>
				</view>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text class="foot-txt">已有账号</text>
				<text class="link" @click="goLogin">返回登录</text>
			</view>
			<text class="copy">© 2026 NEON CITY · NODE 07</text>
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
				phStyle: 'color:rgba(232,236,255,0.42);font-size:15px;',
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
				this.password = 'n30n-5512';
				this.confirm = 'n30n-5512';
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
				else uni.reLaunch({ url: '/pages/login112/login' });
			},

			goLogin() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/login112/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
