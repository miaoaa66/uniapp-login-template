<template>
	<view class="page">
		<view class="deco deco-grid-paper"></view>
		<view class="deco deco-doodle deco-doodle-a">✦</view>
		<view class="deco deco-doodle deco-doodle-b">✧</view>
		<view class="deco deco-doodle deco-doodle-c">○</view>

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
			<view class="cover">
				<view class="cover-tape"></view>
				<text class="cover-title">开一本新账本</text>
				<text class="cover-sub">— 新的记录开始啦 —</text>
			</view>

			<view class="lines">
				<view class="wfield" :class="{ active: focus === 'phone', error: !!err.phone }">
					<text class="wfield-label">手机号</text>
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

				<view class="wfield" :class="{ active: focus === 'sms', error: !!err.sms }">
					<text class="wfield-label">验证码</text>
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

				<view class="wfield" :class="{ active: focus === 'password', error: !!err.password }">
					<text class="wfield-label">设个密码</text>
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

				<view class="wfield" :class="{ active: focus === 'confirm', error: !!err.confirm }">
					<text class="wfield-label">再写一遍</text>
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

				<view class="btn" :class="{ 'is-busy': submitting }" @click="submit">
					<text class="btn-txt">{{ submitting ? '写好了…' : '记好了，提交 ✓' }}</text>
				</view>

				<view class="check" @click="agreed = !agreed">
					<view class="check-box" :class="{ on: agreed }"><text v-if="agreed" class="check-tick">✓</text></view>
					<text class="check-txt">我已阅读并同意 <text class="check-link">《用户协议》</text> 与 <text class="check-link">《隐私政策》</text></text>
				</view>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text class="foot-txt">已经有账本了？</text>
				<text class="link" @click="goLogin">回去登录 →</text>
			</view>
			<text class="copy">♡ 今日也要认真记录 ♡</text>
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
				phStyle: 'color:rgba(58,54,48,0.35);font-size:15px;',
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
				else uni.reLaunch({ url: '/pages/login118/login' });
			},

			goLogin() {
				uni.reLaunch({ url: '/pages/login118/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
