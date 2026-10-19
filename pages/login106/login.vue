<template>
	<view class="mn-page">
		<!-- 状态栏占位，H5 下为 0 -->
		<view class="mn-status" :style="{ height: statusBar + 'px' }"></view>

		<!-- 自绘导航：返回 + 流程序号 -->
		<view class="mn-nav">
			<view class="mn-back" @click="goBack">
				<view class="mn-back__line"></view>
				<view class="mn-back__tip"></view>
				<view class="mn-back__tip mn-back__tip--2"></view>
			</view>
			<text class="mn-nav__idx">01 / 03</text>
		</view>

		<view class="mn-main">
			<view class="mn-head">
				<text class="mn-eyebrow">ACCOUNT · SIGN IN</text>
				<text class="mn-title">登录</text>
				<text class="mn-lead">输入账号信息以继续。首次使用，请先创建一个账号。</text>
			</view>

			<view class="mn-rule"></view>

			<!-- 登录方式 -->
			<view class="mn-tabs">
				<text v-for="(item, i) in tabs" :key="i" class="mn-tab" :class="{ on: tab === i }" @click="switchTab(i)">{{ item }}</text>
			</view>

			<view class="mn-form">
				<!-- 密码登录 -->
				<block v-if="tab === 0">
					<view class="mn-field">
						<view class="mn-field__bar">
							<text class="mn-label">手机号 / 邮箱</text>
							<text class="mn-index">01</text>
						</view>
						<view class="mn-control" :class="{ 'is-focus': focus === 'account', 'is-error': !!err.account }">
							<input
								class="mn-input"
								v-model="account"
								:placeholder-style="phStyle"
								placeholder="请输入手机号或邮箱"
								maxlength="40"
								@focus="focus = 'account'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.account" class="mn-err">{{ err.account }}</text>
					</view>

					<view class="mn-field">
						<view class="mn-field__bar">
							<text class="mn-label">登录密码</text>
							<text class="mn-index">02</text>
						</view>
						<view class="mn-control" :class="{ 'is-focus': focus === 'password', 'is-error': !!err.password }">
							<input
								class="mn-input"
								v-model="password"
								:password="!showPwd"
								:placeholder-style="phStyle"
								placeholder="请输入登录密码"
								maxlength="20"
								@focus="focus = 'password'"
								@blur="focus = ''"
							/>
							<text class="mn-suffix" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
						</view>
						<text v-if="err.password" class="mn-err">{{ err.password }}</text>
					</view>
				</block>

				<!-- 手机验证码 -->
				<block v-else-if="tab === 1">
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
							<text class="mn-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
						</view>
						<text v-if="err.sms" class="mn-err">{{ err.sms }}</text>
					</view>
				</block>

				<!-- 邮箱验证码 -->
				<block v-else>
					<view class="mn-field">
						<view class="mn-field__bar">
							<text class="mn-label">邮箱地址</text>
							<text class="mn-index">01</text>
						</view>
						<view class="mn-control" :class="{ 'is-focus': focus === 'email', 'is-error': !!err.email }">
							<input
								class="mn-input"
								v-model="email"
								:placeholder-style="phStyle"
								placeholder="name@example.com"
								maxlength="40"
								@focus="focus = 'email'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.email" class="mn-err">{{ err.email }}</text>
					</view>

					<view class="mn-field">
						<view class="mn-field__bar">
							<text class="mn-label">邮箱验证码</text>
							<text class="mn-index">02</text>
						</view>
						<view class="mn-control" :class="{ 'is-focus': focus === 'mail', 'is-error': !!err.mail }">
							<input
								class="mn-input mn-input--num"
								v-model="mail"
								type="number"
								:placeholder-style="phStyle"
								placeholder="6 位数字"
								maxlength="6"
								@focus="focus = 'mail'"
								@blur="focus = ''"
							/>
							<text class="mn-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + ' s' : '获取验证码' }}</text>
						</view>
						<text v-if="err.mail" class="mn-err">{{ err.mail }}</text>
					</view>
				</block>

				<view class="mn-row">
					<view class="mn-check mn-check--inline" @click="remember = !remember">
						<view class="mn-check__box" :class="{ on: remember }"><view class="mn-check__tick"></view></view>
						<text class="mn-check__txt">记住此设备</text>
					</view>
					<text class="mn-link mn-link--soft" @click="goForget">忘记密码</text>
				</view>

				<view class="mn-btn" :class="{ 'is-busy': submitting }" @click="submit">登 录</view>

				<view class="mn-check" @click="agreed = !agreed">
					<view class="mn-check__box" :class="{ on: agreed }"><view class="mn-check__tick"></view></view>
					<text class="mn-check__txt">我已阅读并同意 <text class="mn-check__link">《用户协议》</text> 与 <text class="mn-check__link">《隐私政策》</text></text>
				</view>

				<view class="mn-alt">
					<view class="mn-alt__i" @click="toast('微信登录未接入')">微信</view>
					<view class="mn-alt__i" @click="toast('企业微信登录未接入')">企业微信</view>
					<view class="mn-alt__i" @click="toast('单点登录未接入')">SSO</view>
				</view>
			</view>
		</view>

		<view class="mn-foot">
			<view class="mn-foot__row">
				<text class="mn-foot__txt">还没有账号</text>
				<text class="mn-link" @click="goRegister">创建账号</text>
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
				tabs: ['密码', '手机', '邮箱'],
				tab: 0,
				focus: '',
				showPwd: false,
				remember: false,
				agreed: false,
				submitting: false,
				phStyle: 'color:#8A8A8A;font-size:15px;',
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
				this.account = 'hello@minimal.studio';
				this.password = 'blank-2026';
				this.phone = '13800001234';
				this.sms = '202610';
				this.email = 'hello@minimal.studio';
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
				uni.navigateTo({ url: '/pages/login106/register' });
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login106/forget' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
