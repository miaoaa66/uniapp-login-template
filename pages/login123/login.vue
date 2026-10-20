<template>
	<view class="yk-page">
		<!-- 银色渐变天空：泡泡 + 星芒 + 侧边竖标 -->
		<view class="yk-bubble yk-bubble--1"></view>
		<view class="yk-bubble yk-bubble--2"></view>
		<view class="yk-bubble yk-bubble--3"></view>
		<view class="yk-spark yk-spark--1"></view>
		<view class="yk-spark yk-spark--2"></view>
		<view class="yk-spark yk-spark--3"></view>
		<text class="yk-side">Y2K ☆ MILLENNIUM ☆ 2000</text>

		<view class="yk-status" :style="{ height: statusBar + 'px' }"></view>

		<view class="yk-nav">
			<view class="yk-back" @click="goBack">
				<view class="yk-back__line"></view>
				<view class="yk-back__tip"></view>
				<view class="yk-back__tip yk-back__tip--2"></view>
			</view>
			<text class="yk-nav__idx">01 / 03</text>
		</view>

		<view class="yk-main">
			<!-- 悬浮铬卡 -->
			<view class="yk-card">
				<view class="yk-brand">
					<text class="yk-brand__dot"></text>
					<text class="yk-brand__txt">CHROME MAIL · 2000</text>
				</view>
				<text class="yk-title">千 禧 登 录</text>

				<!-- 铬合金胶囊滑块 -->
				<view class="yk-tabs">
					<view class="yk-thumb" :style="{ transform: 'translateX(' + tab * 100 + '%)' }"></view>
					<text v-for="(item, i) in tabs" :key="i" class="yk-tab" :class="{ on: tab === i }" @click="switchTab(i)">{{ item }}</text>
				</view>

				<view class="yk-form">
					<!-- 密码登录 -->
					<block v-if="tab === 0">
						<view class="yk-field">
							<text class="yk-label"><text class="yk-label__dot"></text>ACCOUNT · 手机号 / 邮箱</text>
							<view class="yk-control" :class="{ 'is-focus': focus === 'account', 'is-error': !!err.account }">
								<input
									class="yk-input"
									v-model="account"
									:placeholder-style="phStyle"
									placeholder="user@millennium.net"
									maxlength="40"
									@focus="focus = 'account'"
									@blur="focus = ''"
								/>
							</view>
							<text v-if="err.account" class="yk-err">{{ err.account }}</text>
						</view>

						<view class="yk-field">
							<text class="yk-label"><text class="yk-label__dot yk-label__dot--pink"></text>PASSWORD · 登录密码</text>
							<view class="yk-control" :class="{ 'is-focus': focus === 'password', 'is-error': !!err.password }">
								<input
									class="yk-input"
									v-model="password"
									:password="!showPwd"
									:placeholder-style="phStyle"
									placeholder="••••••••"
									maxlength="20"
									@focus="focus = 'password'"
									@blur="focus = ''"
								/>
								<text class="yk-suffix" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
							</view>
							<text v-if="err.password" class="yk-err">{{ err.password }}</text>
						</view>
					</block>

					<!-- 手机验证码 -->
					<block v-else-if="tab === 1">
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
								<text class="yk-suffix yk-suffix--code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
							</view>
							<text v-if="err.sms" class="yk-err">{{ err.sms }}</text>
						</view>
					</block>

					<!-- 邮箱验证码 -->
					<block v-else>
						<view class="yk-field">
							<text class="yk-label"><text class="yk-label__dot"></text>EMAIL · 邮箱地址</text>
							<view class="yk-control" :class="{ 'is-focus': focus === 'email', 'is-error': !!err.email }">
								<input
									class="yk-input"
									v-model="email"
									:placeholder-style="phStyle"
									placeholder="name@millennium.net"
									maxlength="40"
									@focus="focus = 'email'"
									@blur="focus = ''"
								/>
							</view>
							<text v-if="err.email" class="yk-err">{{ err.email }}</text>
						</view>

						<view class="yk-field">
							<text class="yk-label"><text class="yk-label__dot yk-label__dot--pink"></text>CODE · 邮箱验证码</text>
							<view class="yk-control" :class="{ 'is-focus': focus === 'mail', 'is-error': !!err.mail }">
								<input
									class="yk-input yk-input--num"
									v-model="mail"
									type="number"
									:placeholder-style="phStyle"
									placeholder="6 位数字"
									maxlength="6"
									@focus="focus = 'mail'"
									@blur="focus = ''"
								/>
								<text class="yk-suffix yk-suffix--code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + ' s' : '获取验证码' }}</text>
							</view>
							<text v-if="err.mail" class="yk-err">{{ err.mail }}</text>
						</view>
					</block>

					<view class="yk-row">
						<view class="yk-check yk-check--inline" @click="remember = !remember">
							<view class="yk-check__box" :class="{ on: remember }"><view class="yk-check__tick"></view></view>
							<text class="yk-check__txt">记住这台机器</text>
						</view>
						<text class="yk-link yk-link--soft" @click="goForget">忘记密码？</text>
					</view>

					<!-- 金属扫光按钮 -->
					<view class="yk-btn" :class="{ 'is-busy': submitting }" @click="submit">连 线 2000</view>

					<view class="yk-check" @click="agreed = !agreed">
						<view class="yk-check__box" :class="{ on: agreed }"><view class="yk-check__tick"></view></view>
						<text class="yk-check__txt">我已阅读并同意 <text class="yk-check__link">《用户协议》</text> 与 <text class="yk-check__link">《隐私政策》</text></text>
					</view>

					<view class="yk-alt">
						<view class="yk-alt__i" @click="toast('微信登录未接入')">微信</view>
						<view class="yk-alt__i" @click="toast('企业微信登录未接入')">企业微信</view>
						<view class="yk-alt__i" @click="toast('单点登录未接入')">SSO</view>
					</view>
				</view>
			</view>
		</view>

		<view class="yk-foot">
			<view class="yk-foot__row">
				<text class="yk-foot__txt">还没注册？</text>
				<text class="yk-link" @click="goRegister">申请一个新号码 →</text>
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
				tabs: ['密码', '手机', '邮箱'],
				tab: 0,
				focus: '',
				showPwd: false,
				remember: false,
				agreed: false,
				submitting: false,
				phStyle: 'color:#8FA2B2;font-size:15px;',
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
			fillDemo() {
				this.agreed = true;
				this.account = 'y2k@chrome.fm';
				this.password = 'chrome-2000';
				this.phone = '13800001234';
				this.sms = '202610';
				this.email = 'y2k@chrome.fm';
				this.mail = '202610';
			},

			toast(title) {
				uni.showToast({ title, icon: 'none' });
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
				uni.navigateTo({ url: '/pages/login123/register' });
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login123/forget' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
