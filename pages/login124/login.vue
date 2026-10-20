<template>
	<view class="vp-page">
		<!-- 天地分屏：上 = 渐变天空 + 半出画面的落日；下 = 透视网格地平线 -->
		<view class="vp-sky">
			<view class="vp-sun"></view>
			<view class="vp-star vp-star--1"></view>
			<view class="vp-star vp-star--2"></view>
			<view class="vp-star vp-star--3"></view>
			<text class="vp-kana">ヴェイパーウェイヴ · FM 89.0</text>
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
			<text class="vp-nav__idx">01 / 03</text>
		</view>

		<!-- 跨在天与地之间的控制面板 -->
		<view class="vp-panel">
			<view class="vp-head">
				<text class="vp-eyebrow">VAPORWAVE FM · 深夜频段</text>
				<text class="vp-title">蒸汽波电台</text>
				<text class="vp-lead">调到 89.0 兆赫，输入凭证，进入慢速梦境。</text>
			</view>

			<!-- 霓虹 chip 切换 -->
			<view class="vp-chips">
				<text v-for="(item, i) in tabs" :key="i" class="vp-chip" :class="{ on: tab === i }" @click="switchTab(i)">{{ item }}</text>
			</view>

			<view class="vp-form">
				<!-- 密码登录 -->
				<block v-if="tab === 0">
					<view class="vp-field">
						<text class="vp-label">ACCOUNT / 手机号 或 邮箱</text>
						<view class="vp-control" :class="{ 'is-focus': focus === 'account', 'is-error': !!err.account }">
							<input
								class="vp-input"
								v-model="account"
								:placeholder-style="phStyle"
								placeholder="dreamer@vapor.fm"
								maxlength="40"
								@focus="focus = 'account'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.account" class="vp-err">{{ err.account }}</text>
					</view>

					<view class="vp-field">
						<text class="vp-label">PASSWORD / 登录密码</text>
						<view class="vp-control" :class="{ 'is-focus': focus === 'password', 'is-error': !!err.password }">
							<input
								class="vp-input"
								v-model="password"
								:password="!showPwd"
								:placeholder-style="phStyle"
								placeholder="••••••••"
								maxlength="20"
								@focus="focus = 'password'"
								@blur="focus = ''"
							/>
							<text class="vp-suffix" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
						</view>
						<text v-if="err.password" class="vp-err">{{ err.password }}</text>
					</view>
				</block>

				<!-- 手机验证码 -->
				<block v-else-if="tab === 1">
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
							<text class="vp-suffix vp-suffix--code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
						</view>
						<text v-if="err.sms" class="vp-err">{{ err.sms }}</text>
					</view>
				</block>

				<!-- 邮箱验证码 -->
				<block v-else>
					<view class="vp-field">
						<text class="vp-label">EMAIL / 邮箱地址</text>
						<view class="vp-control" :class="{ 'is-focus': focus === 'email', 'is-error': !!err.email }">
							<input
								class="vp-input"
								v-model="email"
								:placeholder-style="phStyle"
								placeholder="dreamer@vapor.fm"
								maxlength="40"
								@focus="focus = 'email'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.email" class="vp-err">{{ err.email }}</text>
					</view>

					<view class="vp-field">
						<text class="vp-label">CODE / 邮箱验证码</text>
						<view class="vp-control" :class="{ 'is-focus': focus === 'mail', 'is-error': !!err.mail }">
							<input
								class="vp-input vp-input--num"
								v-model="mail"
								type="number"
								:placeholder-style="phStyle"
								placeholder="6 位数字"
								maxlength="6"
								@focus="focus = 'mail'"
								@blur="focus = ''"
							/>
							<text class="vp-suffix vp-suffix--code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + ' s' : '获取验证码' }}</text>
						</view>
						<text v-if="err.mail" class="vp-err">{{ err.mail }}</text>
					</view>
				</block>

				<view class="vp-row">
					<view class="vp-check vp-check--inline" @click="remember = !remember">
						<view class="vp-check__box" :class="{ on: remember }"><view class="vp-check__tick"></view></view>
						<text class="vp-check__txt">在这台机器上做梦</text>
					</view>
					<text class="vp-link vp-link--soft" @click="goForget">忘记密码</text>
				</view>

				<!-- 粉紫渐变按钮 -->
				<view class="vp-btn" :class="{ 'is-busy': submitting }" @click="submit">接 入 信 道</view>

				<view class="vp-check" @click="agreed = !agreed">
					<view class="vp-check__box" :class="{ on: agreed }"><view class="vp-check__tick"></view></view>
					<text class="vp-check__txt">我已阅读并同意 <text class="vp-check__link">《用户协议》</text> 与 <text class="vp-check__link">《隐私政策》</text></text>
				</view>

				<view class="vp-alt">
					<view class="vp-alt__i" @click="toast('微信登录未接入')">微信</view>
					<view class="vp-alt__i" @click="toast('企业微信登录未接入')">企业微信</view>
					<view class="vp-alt__i" @click="toast('单点登录未接入')">SSO</view>
				</view>
			</view>
		</view>

		<view class="vp-foot">
			<view class="vp-foot__row">
				<text class="vp-foot__txt">还没有听众账号</text>
				<text class="vp-link" @click="goRegister">申请入场券 →</text>
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
				tabs: ['密码', '手机', '邮箱'],
				tab: 0,
				focus: '',
				showPwd: false,
				remember: false,
				agreed: false,
				submitting: false,
				phStyle: 'color:#6E64A0;font-size:15px;',
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
				this.account = 'vapor@fm.jp';
				this.password = 'neon-1989';
				this.phone = '13800001234';
				this.sms = '202610';
				this.email = 'vapor@fm.jp';
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
				uni.navigateTo({ url: '/pages/login124/register' });
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login124/forget' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
