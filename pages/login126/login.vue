<template>
	<view class="sw-page">
		<view class="sw-grid">
			<!-- 改造新增：聚焦时对应网格栏高亮条 -->
			<view class="sw-grid__hl" :class="{ show: gridCol >= 0 }" :style="{ transform: 'translateX(' + (gridCol >= 0 ? gridCol * 100 : 0) + '%)' }"></view>
		</view>
		<view class="sw-status" :style="{ height: statusBar + 'px' }"></view>

		<view class="sw-top">
			<view class="sw-back" @click="goBack">
				<view class="sw-back__arrow"></view>
			</view>
			<text class="sw-top__meta">SWISS GRID SYSTEM · 12 COL / CH</text>
		</view>
		<view class="sw-rule"></view>

		<!-- 标题区 -->
		<view class="sw-hero">
			<view class="sw-hero__accent"></view>
			<text class="sw-hero__title">登录</text>
			<text class="sw-hero__en">SIGN IN / ANMELDEN</text>
			<text class="sw-hero__desc">秩序、留白与字重，构成全部界面。请选择一种方式验证身份。</text>
		</view>

		<!-- 登录方式 -->
		<view class="sw-tabs">
			<view v-for="(item, i) in tabs" :key="i" class="sw-tab" :class="{ on: tab === i }" @click="switchTab(i)">
				<text class="sw-tab__num">0{{ i + 1 }}</text>
				<text class="sw-tab__txt">{{ item }}</text>
			</view>
			<!-- 改造新增：红色方块滑动指示器 -->
			<view class="sw-tabs__ind" :style="{ transform: 'translateX(' + tab * 100 + '%)' }"><view class="sw-tabs__sq"></view></view>
		</view>

		<view class="sw-form">
			<!-- 密码登录 -->
			<block v-if="tab === 0">
				<view
					class="sw-field"
					:class="{ 'is-focus': focus === 'account', 'is-on': focus === 'account' || !!account, 'is-error': !!err.account }"
				>
					<text class="sw-field__label">账号<text class="sw-field__req">*</text> / ACCOUNT</text>
					<input
						class="sw-field__input"
						v-model="account"
						:placeholder-style="phStyle"
						placeholder=""
						maxlength="40"
						@focus="focus = 'account'; gridCol = 1"
						@blur="focus = ''; gridCol = -1"
					/>
					<view class="sw-field__line"><view class="sw-field__ink"></view></view>
				</view>
				<text v-if="err.account" class="sw-err">{{ err.account }}</text>

				<view
					class="sw-field sw-field--toggle"
					:class="{ 'is-focus': focus === 'password', 'is-on': focus === 'password' || !!password, 'is-error': !!err.password }"
				>
					<text class="sw-field__label">密码<text class="sw-field__req">*</text> / PASSWORD</text>
					<input
						class="sw-field__input"
						v-model="password"
						:password="!showPwd"
						:placeholder-style="phStyle"
						placeholder=""
						maxlength="20"
						@focus="focus = 'password'; gridCol = 3"
						@blur="focus = ''; gridCol = -1"
					/>
					<text class="sw-field__toggle" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
					<view class="sw-field__line"><view class="sw-field__ink"></view></view>
				</view>
				<text v-if="err.password" class="sw-err">{{ err.password }}</text>
			</block>

			<!-- 手机验证码 -->
			<block v-else-if="tab === 1">
				<view
					class="sw-field"
					:class="{ 'is-focus': focus === 'phone', 'is-on': focus === 'phone' || !!phone, 'is-error': !!err.phone }"
				>
					<text class="sw-field__label">手机号<text class="sw-field__req">*</text> / MOBILE</text>
					<input
						class="sw-field__input"
						v-model="phone"
						type="number"
						:placeholder-style="phStyle"
						placeholder=""
						maxlength="11"
						@focus="focus = 'phone'; gridCol = 2"
						@blur="focus = ''; gridCol = -1"
					/>
					<view class="sw-field__line"><view class="sw-field__ink"></view></view>
				</view>
				<text v-if="err.phone" class="sw-err">{{ err.phone }}</text>

				<view
					class="sw-field sw-field--code"
					:class="{ 'is-focus': focus === 'sms', 'is-on': focus === 'sms' || !!sms, 'is-error': !!err.sms }"
				>
					<text class="sw-field__label">验证码<text class="sw-field__req">*</text> / SMS CODE</text>
					<input
						class="sw-field__input"
						v-model="sms"
						type="number"
						:placeholder-style="phStyle"
						placeholder=""
						maxlength="6"
						@focus="focus = 'sms'; gridCol = 5"
						@blur="focus = ''; gridCol = -1"
					/>
					<text class="sw-field__code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
					<view class="sw-field__line"><view class="sw-field__ink"></view></view>
				</view>
				<text v-if="err.sms" class="sw-err">{{ err.sms }}</text>
			</block>

			<!-- 邮箱验证码 -->
			<block v-else>
				<view
					class="sw-field"
					:class="{ 'is-focus': focus === 'email', 'is-on': focus === 'email' || !!email, 'is-error': !!err.email }"
				>
					<text class="sw-field__label">邮箱<text class="sw-field__req">*</text> / E-MAIL</text>
					<input
						class="sw-field__input"
						v-model="email"
						:placeholder-style="phStyle"
						placeholder=""
						maxlength="40"
						@focus="focus = 'email'; gridCol = 1"
						@blur="focus = ''; gridCol = -1"
					/>
					<view class="sw-field__line"><view class="sw-field__ink"></view></view>
				</view>
				<text v-if="err.email" class="sw-err">{{ err.email }}</text>

				<view
					class="sw-field sw-field--code"
					:class="{ 'is-focus': focus === 'mail', 'is-on': focus === 'mail' || !!mail, 'is-error': !!err.mail }"
				>
					<text class="sw-field__label">验证码<text class="sw-field__req">*</text> / MAIL CODE</text>
					<input
						class="sw-field__input"
						v-model="mail"
						type="number"
						:placeholder-style="phStyle"
						placeholder=""
						maxlength="6"
						@focus="focus = 'mail'; gridCol = 3"
						@blur="focus = ''; gridCol = -1"
					/>
					<text class="sw-field__code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + ' s' : '获取验证码' }}</text>
					<view class="sw-field__line"><view class="sw-field__ink"></view></view>
				</view>
				<text v-if="err.mail" class="sw-err">{{ err.mail }}</text>
			</block>

			<view class="sw-row">
				<view class="sw-check" @click="remember = !remember">
					<view class="sw-check__box" :class="{ on: remember }"><view class="sw-check__tick"></view></view>
					<text class="sw-check__txt">保持登录状态</text>
				</view>
				<text class="sw-link sw-link--dim" @click="goForget">忘记密码</text>
			</view>

			<view class="sw-submit" :class="{ 'is-busy': submitting }" @click="submit">
				<text class="sw-submit__txt">登 录</text>
				<view class="sw-submit__right">
					<text class="sw-submit__en">SIGN IN</text>
					<text class="sw-submit__arrow">→</text>
				</view>
			</view>

			<view class="sw-agree" @click="agreed = !agreed">
				<view class="sw-check__box" :class="{ on: agreed }"><view class="sw-check__tick"></view></view>
				<text class="sw-agree__txt">我已阅读并同意 <text class="sw-agree__link">《用户协议》</text> 与 <text class="sw-agree__link">《隐私政策》</text></text>
			</view>

			<view class="sw-div">
				<view class="sw-div__line"></view>
				<text class="sw-div__txt">WEITERE / 其他方式</text>
				<view class="sw-div__line"></view>
			</view>

			<view class="sw-alt">
				<view class="sw-alt__i" @click="toast('微信登录未接入')">微信</view>
				<view class="sw-alt__i" @click="toast('企业微信未接入')">企业微信</view>
				<view class="sw-alt__i" @click="toast('单点登录未接入')">SSO</view>
			</view>
		</view>

		<view class="sw-foot">
			<view class="sw-foot__rule"></view>
			<view class="sw-foot__body">
				<text class="sw-foot__txt">还没有账号？<text class="sw-link" @click="goRegister">立即注册</text></text>
			</view>
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
				gridCol: -1,
				showPwd: false,
				remember: false,
				agreed: false,
				submitting: false,
				phStyle: 'color:#A7A39B;font-size:30rpx;',
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
				this.account = 'hello@grid.ch';
				this.password = 'helvetica-1957';
				this.phone = '13800001234';
				this.sms = '126126';
				this.email = 'hello@grid.ch';
				this.mail = '126126';
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
				uni.navigateTo({ url: '/pages/login126/register' });
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login126/forget' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
