<template>
	<view class="page">
		<view class="deco deco-circle"></view>
		<view class="deco deco-tri"></view>
		<view class="deco deco-bar"></view>
		<view class="deco deco-rule"></view>

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
			<view class="mark">
				<view class="mark-sq"></view>
				<view class="mark-tri"></view>
				<view class="mark-cir"></view>
			</view>

			<view class="head">
				<text class="title">创建账号</text>
				<text class="lead">登记新的成员档案，加入设计公社。</text>
			</view>

			<view class="forms">
				<view class="gfield" :class="{ active: focus === 'phone', error: !!err.phone }">
					<view class="gfield-dot gfield-dot-y"></view>
					<view class="gfield-main">
						<text class="gfield-label">手机号</text>
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
					<view class="focus-dot"></view>
				</view>
				<text v-if="err.phone" class="err">{{ err.phone }}</text>

				<view class="gfield" :class="{ active: focus === 'sms', error: !!err.sms }">
					<view class="gfield-dot gfield-dot-b"></view>
					<view class="gfield-main">
						<text class="gfield-label">短信验证码</text>
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
					</view>
					<text class="code" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
					<view class="focus-dot"></view>
				</view>
				<text v-if="err.sms" class="err">{{ err.sms }}</text>

				<view class="gfield" :class="{ active: focus === 'password', error: !!err.password }">
					<view class="gfield-dot gfield-dot-y"></view>
					<view class="gfield-main">
						<text class="gfield-label">设置密码</text>
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
					</view>
					<text class="eye" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
					<view class="focus-dot"></view>
				</view>
				<text v-if="err.password" class="err">{{ err.password }}</text>

				<view class="gfield" :class="{ active: focus === 'confirm', error: !!err.confirm }">
					<view class="gfield-dot gfield-dot-b"></view>
					<view class="gfield-main">
						<text class="gfield-label">确认密码</text>
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
					<view class="focus-dot"></view>
				</view>
				<text v-if="err.confirm" class="err">{{ err.confirm }}</text>

				<view class="btn" :class="{ 'is-busy': submitting }" @click="submit">
					<text class="btn-txt">{{ submitting ? '提交中…' : '注　册' }}</text>
				</view>

				<view class="check" @click="agreed = !agreed">
					<view class="check-box" :class="{ on: agreed }"><view v-if="agreed" class="check-tick"></view></view>
					<text class="check-txt">我已阅读并同意 <text class="check-link">《用户协议》</text> 与 <text class="check-link">《隐私政策》</text></text>
				</view>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text class="foot-txt">已有账号</text>
				<text class="link" @click="goLogin">返回登录</text>
			</view>
			<text class="copy">© 2026 STAATLICHES BAUHAUS · 1926</text>
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
				phStyle: 'color:rgba(17,17,17,0.34);font-size:15px;',
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
			toast(title) {
				uni.showToast({ title, icon: 'none' });
			},

			fillDemo() {
				this.phone = '13800001234';
				this.sms = '202610';
				this.password = 'demo1234';
				this.confirm = 'demo1234';
				this.agreed = true;
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
				else uni.reLaunch({ url: '/pages/login117/login' });
			},

			goLogin() {
				uni.reLaunch({ url: '/pages/login117/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
