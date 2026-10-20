<template>
	<view class="og-page">
		<!-- 顶部绿色弧冠：标题住在弧里，卡片压弧而出 -->
		<view class="og-crown">
			<view class="og-crown__leaf og-crown__leaf--l"></view>
			<view class="og-crown__leaf og-crown__leaf--r"></view>
			<view class="og-status" :style="{ height: statusBar + 'px' }"></view>

			<view class="og-nav">
				<view class="og-back" @click="goBack">
					<view class="og-back__line"></view>
					<view class="og-back__tip"></view>
					<view class="og-back__tip og-back__tip--2"></view>
				</view>
				<text class="og-nav__idx">01 / 03</text>
			</view>

			<view class="og-crown__body">
				<text class="og-eyebrow">ORGANIC GROVE · 温柔入口</text>
				<text class="og-title">早安，回来啦</text>
				<text class="og-lead">深呼吸，输入账号信息，我们带你回到绿洲。</text>
			</view>
		</view>

		<view class="og-main">
			<!-- 悬浮卡片 -->
			<view class="og-card">
				<!-- 登录方式：叶形分段器 -->
				<view class="og-leaves">
					<text
						v-for="(item, i) in tabs"
						:key="i"
						class="og-leaf"
						:class="{ on: tab === i }"
						@click="switchTab(i)"
					>{{ item }}</text>
				</view>

				<view class="og-form">
					<!-- 密码登录 -->
					<block v-if="tab === 0">
						<view class="og-field" :class="{ 'is-float': focus === 'account' || account, 'is-error': !!err.account }">
							<text class="og-label">手机号 / 邮箱</text>
							<input
								class="og-input"
								v-model="account"
								:placeholder-style="phStyle"
								placeholder=" "
								maxlength="40"
								@focus="focus = 'account'"
								@blur="focus = ''"
							/>
							<text v-if="err.account" class="og-err">{{ err.account }}</text>
						</view>

						<view class="og-field" :class="{ 'is-float': focus === 'password' || password, 'is-error': !!err.password }">
							<text class="og-label">登录密码</text>
							<view class="og-inputrow">
								<input
									class="og-input"
									v-model="password"
									:password="!showPwd"
									:placeholder-style="phStyle"
									placeholder=" "
									maxlength="20"
									@focus="focus = 'password'"
									@blur="focus = ''"
								/>
								<text class="og-suffix" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
							</view>
							<text v-if="err.password" class="og-err">{{ err.password }}</text>
						</view>
					</block>

					<!-- 手机验证码 -->
					<block v-else-if="tab === 1">
						<view class="og-field" :class="{ 'is-float': focus === 'phone' || phone, 'is-error': !!err.phone }">
							<text class="og-label">手机号</text>
							<input
								class="og-input og-input--num"
								v-model="phone"
								type="number"
								:placeholder-style="phStyle"
								placeholder=" "
								maxlength="11"
								@focus="focus = 'phone'"
								@blur="focus = ''"
							/>
							<text v-if="err.phone" class="og-err">{{ err.phone }}</text>
						</view>

						<view class="og-field" :class="{ 'is-float': focus === 'sms' || sms, 'is-error': !!err.sms }">
							<text class="og-label">短信验证码</text>
							<view class="og-inputrow">
								<input
									class="og-input og-input--num"
									v-model="sms"
									type="number"
									:placeholder-style="phStyle"
									placeholder=" "
									maxlength="6"
									@focus="focus = 'sms'"
									@blur="focus = ''"
								/>
								<text class="og-suffix og-suffix--code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
							</view>
							<text v-if="err.sms" class="og-err">{{ err.sms }}</text>
						</view>
					</block>

					<!-- 邮箱验证码 -->
					<block v-else>
						<view class="og-field" :class="{ 'is-float': focus === 'email' || email, 'is-error': !!err.email }">
							<text class="og-label">邮箱地址</text>
							<input
								class="og-input"
								v-model="email"
								:placeholder-style="phStyle"
								placeholder=" "
								maxlength="40"
								@focus="focus = 'email'"
								@blur="focus = ''"
							/>
							<text v-if="err.email" class="og-err">{{ err.email }}</text>
						</view>

						<view class="og-field" :class="{ 'is-float': focus === 'mail' || mail, 'is-error': !!err.mail }">
							<text class="og-label">邮箱验证码</text>
							<view class="og-inputrow">
								<input
									class="og-input og-input--num"
									v-model="mail"
									type="number"
									:placeholder-style="phStyle"
									placeholder=" "
									maxlength="6"
									@focus="focus = 'mail'"
									@blur="focus = ''"
								/>
								<text class="og-suffix og-suffix--code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + ' s' : '获取验证码' }}</text>
							</view>
							<text v-if="err.mail" class="og-err">{{ err.mail }}</text>
						</view>
					</block>

					<view class="og-row">
						<view class="og-check og-check--inline" @click="remember = !remember">
							<view class="og-check__box" :class="{ on: remember }"><view class="og-check__tick"></view></view>
							<text class="og-check__txt">记住这台设备</text>
						</view>
						<text class="og-link og-link--soft" @click="goForget">忘记密码</text>
					</view>

					<!-- 叶形主按钮 -->
					<view class="og-btn" :class="{ 'is-busy': submitting }" @click="submit">进 入 绿 洲</view>

					<view class="og-check" @click="agreed = !agreed">
						<view class="og-check__box" :class="{ on: agreed }"><view class="og-check__tick"></view></view>
						<text class="og-check__txt">我已阅读并同意 <text class="og-check__link">《用户协议》</text> 与 <text class="og-check__link">《隐私政策》</text></text>
					</view>

					<view class="og-alt">
						<text class="og-alt__hint">其他方式，一样自然</text>
						<view class="og-alt__row">
							<view class="og-alt__i" @click="toast('微信登录未接入')">微信</view>
							<view class="og-alt__i" @click="toast('企业微信登录未接入')">企业微信</view>
							<view class="og-alt__i" @click="toast('单点登录未接入')">SSO</view>
						</view>
					</view>
				</view>
			</view>
		</view>

		<!-- 底部弧形草坡 -->
		<view class="og-foot">
			<view class="og-foot__hill">
				<view class="og-foot__row">
					<text class="og-foot__txt">还没有账号</text>
					<text class="og-link" @click="goRegister">种下一颗种子 →</text>
				</view>
				<text class="og-copy">© 2026 ORGANIC GROVE · 慢慢生长</text>
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
				showPwd: false,
				remember: false,
				agreed: false,
				submitting: false,
				phStyle: 'color:#9AA88F;font-size:15px;',
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
				this.account = 'leaf@grove.eco';
				this.password = 'organic-7211';
				this.phone = '13800001234';
				this.sms = '202610';
				this.email = 'leaf@grove.eco';
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
				uni.navigateTo({ url: '/pages/login122/register' });
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login122/forget' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
