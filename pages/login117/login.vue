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
			<text class="nav-idx">01 / 03</text>
		</view>

		<view class="content">
			<view class="mark">
				<view class="mark-sq"></view>
				<view class="mark-tri"></view>
				<view class="mark-cir"></view>
			</view>

			<view class="head">
				<text class="title">登录</text>
				<text class="lead">形式追随功能。输入凭证，进入工作台。</text>
			</view>

			<view class="shapes">
				<view v-for="(item, i) in tabs" :key="i" class="shape" :class="['shape-' + i, { on: tab === i }]" @click="switchTab(i)">
					<view class="shape-fig">
						<view class="fig fig-sq"></view>
						<view class="fig fig-tri"></view>
						<view class="fig fig-cir"></view>
					</view>
					<text class="shape-name">{{ item }}</text>
				</view>
			</view>

			<view class="forms">
				<block v-if="tab === 0">
					<view class="gfield" :class="{ active: focus === 'account', error: !!err.account }">
						<view class="gfield-dot gfield-dot-y"></view>
						<view class="gfield-main">
							<text class="gfield-label">手机号 / 邮箱</text>
							<input
								class="input input-num"
								v-model="account"
								:placeholder-style="phStyle"
								placeholder="请输入手机号或邮箱"
								maxlength="40"
								@focus="focus = 'account'"
								@blur="focus = ''"
							/>
						</view>
						<view class="focus-dot"></view>
					</view>
					<text v-if="err.account" class="err">{{ err.account }}</text>

					<view class="gfield" :class="{ active: focus === 'password', error: !!err.password }">
						<view class="gfield-dot gfield-dot-b"></view>
						<view class="gfield-main">
							<text class="gfield-label">登录密码</text>
							<input
								class="input"
								v-model="password"
								:password="!showPwd"
								:placeholder-style="phStyle"
								placeholder="请输入登录密码"
								maxlength="20"
								@focus="focus = 'password'"
								@blur="focus = ''"
							/>
						</view>
						<text class="eye" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
						<view class="focus-dot"></view>
					</view>
					<text v-if="err.password" class="err">{{ err.password }}</text>
				</block>

				<block v-else-if="tab === 1">
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
						<text class="code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
						<view class="focus-dot"></view>
					</view>
					<text v-if="err.sms" class="err">{{ err.sms }}</text>
				</block>

				<block v-else>
					<view class="gfield" :class="{ active: focus === 'email', error: !!err.email }">
						<view class="gfield-dot gfield-dot-y"></view>
						<view class="gfield-main">
							<text class="gfield-label">邮箱地址</text>
							<input
								class="input"
								v-model="email"
								:placeholder-style="phStyle"
								placeholder="name@example.com"
								maxlength="40"
								@focus="focus = 'email'"
								@blur="focus = ''"
							/>
						</view>
						<view class="focus-dot"></view>
					</view>
					<text v-if="err.email" class="err">{{ err.email }}</text>

					<view class="gfield" :class="{ active: focus === 'mail', error: !!err.mail }">
						<view class="gfield-dot gfield-dot-b"></view>
						<view class="gfield-main">
							<text class="gfield-label">邮箱验证码</text>
							<input
								class="input input-num"
								v-model="mail"
								type="number"
								:placeholder-style="phStyle"
								placeholder="6 位数字"
								maxlength="6"
								@focus="focus = 'mail'"
								@blur="focus = ''"
							/>
						</view>
						<text class="code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + ' s' : '获取验证码' }}</text>
						<view class="focus-dot"></view>
					</view>
					<text v-if="err.mail" class="err">{{ err.mail }}</text>
				</block>

				<view class="row">
					<view class="check check-inline" @click="remember = !remember">
						<view class="check-box" :class="{ on: remember }"><view v-if="remember" class="check-tick"></view></view>
						<text class="check-txt">记住此设备</text>
					</view>
					<text class="link link-soft" @click="goForget">忘记密码</text>
				</view>

				<view class="btn" :class="{ 'is-busy': submitting }" @click="submit">
					<text class="btn-txt">{{ submitting ? '提交中…' : '登　录' }}</text>
				</view>

				<view class="check" @click="agreed = !agreed">
					<view class="check-box" :class="{ on: agreed }"><view v-if="agreed" class="check-tick"></view></view>
					<text class="check-txt">我已阅读并同意 <text class="check-link">《用户协议》</text> 与 <text class="check-link">《隐私政策》</text></text>
				</view>

				<view class="alt">
					<view class="alt-i" @click="toast('微信登录未接入')">微信</view>
					<view class="alt-i" @click="toast('企业微信登录未接入')">企业微信</view>
					<view class="alt-i" @click="toast('单点登录未接入')">SSO</view>
				</view>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text class="foot-txt">还没有账号</text>
				<text class="link" @click="goRegister">创建账号</text>
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
				tabs: ['密码', '手机', '邮箱'],
				tab: 0,
				focus: '',
				showPwd: false,
				remember: false,
				agreed: false,
				submitting: false,
				phStyle: 'color:rgba(17,17,17,0.34);font-size:15px;',
				account: '',
				password: '',
				phone: '',
				sms: '',
				email: '',
				mail: '',
				smsLeft: 0,
				mailLeft: 0,
				timer: null,
				err: {}
			};
		},
		onLoad(options) {
			const info = uni.getSystemInfoSync();
			this.statusBar = info.statusBarHeight || 0;
			if (options && options.tab) this.tab = Number(options.tab) || 0;
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
				this.account = '13800001234';
				this.password = 'demo1234';
				this.phone = '13800001234';
				this.sms = '202610';
				this.email = 'demo@work.com';
				this.mail = '202610';
			},

			isPhone(v) {
				return /^1\d{10}$/.test(v);
			},

			isEmail(v) {
				return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v);
			},

			switchTab(i) {
				this.tab = i;
				this.err = {};
				this.focus = '';
			},

			sendCode(type) {
				if (type === 'sms') {
					if (this.smsLeft > 0) return;
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号');
					this.smsLeft = 60;
				} else {
					if (this.mailLeft > 0) return;
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱');
					this.mailLeft = 60;
				}
				// TODO: 接入后端验证码接口
				this.toast('验证码已发送');
				this.startTimer();
			},

			startTimer() {
				if (this.timer) return;
				this.timer = setInterval(() => {
					if (this.smsLeft > 0) this.smsLeft--;
					if (this.mailLeft > 0) this.mailLeft--;
					if (this.smsLeft <= 0 && this.mailLeft <= 0) this.clearTimer();
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
				if (this.tab === 0) {
					if (!this.account) e.account = '请输入手机号或邮箱';
					else if (this.account.indexOf('@') > -1 && !this.isEmail(this.account)) e.account = '邮箱格式不正确';
					else if (this.account.indexOf('@') === -1 && !this.isPhone(this.account)) e.account = '手机号格式不正确';
					if (!this.password) e.password = '请输入登录密码';
					else if (this.password.length < 6) e.password = '密码不少于 6 位';
				} else if (this.tab === 1) {
					if (!this.isPhone(this.phone)) e.phone = '请输入正确的手机号';
					if (!this.sms) e.sms = '请输入短信验证码';
				} else {
					if (!this.isEmail(this.email)) e.email = '请输入正确的邮箱地址';
					if (!this.mail) e.mail = '请输入邮箱验证码';
				}
				this.err = e;
				return Object.keys(e).length === 0;
			},

			submit() {
				if (this.submitting) return;
				if (!this.agreed) return this.toast('请先阅读并同意用户协议');
				if (!this.validate()) return;
				this.submitting = true;
				// TODO: 调用后端登录接口，成功后跳转首页
				setTimeout(() => {
					this.submitting = false;
					this.toast('登录成功');
				}, 600);
			},

			goBack() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/index' });
			},

			goRegister() {
				uni.navigateTo({ url: '/pages/login117/register' });
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login117/forget' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
