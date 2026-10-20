<template>
	<view class="dd-page">
		<!-- 手账纸：装订线 + 打孔 + 涂鸦装饰 -->
		<view class="dd-margin"></view>
		<view class="dd-holes">
			<view class="dd-hole"></view>
			<view class="dd-hole"></view>
			<view class="dd-hole"></view>
		</view>
		<view class="dd-deco dd-deco--star"></view>
		<view class="dd-deco dd-deco--wave"></view>
		<view class="dd-deco dd-deco--cross"></view>

		<view class="dd-status" :style="{ height: statusBar + 'px' }"></view>

		<view class="dd-nav">
			<view class="dd-back" @click="goBack">
				<view class="dd-back__line"></view>
				<view class="dd-back__tip"></view>
				<view class="dd-back__tip dd-back__tip--2"></view>
			</view>
			<text class="dd-nav__idx">01 / 03</text>
		</view>

		<view class="dd-main">
			<view class="dd-head">
				<text class="dd-eyebrow">MY LITTLE NOTEBOOK · 今天的第一页</text>
				<text class="dd-title">手账登录</text>
				<text class="dd-lead">像写日记一样，把账号轻轻写进小本本里。</text>
			</view>

			<!-- 便签卡：胶带贴住，微微歪斜 -->
			<view class="dd-note">
				<view class="dd-note__tape dd-note__tape--l"></view>
				<view class="dd-note__tape dd-note__tape--r"></view>

				<!-- 登录方式：三张歪斜贴纸 -->
				<view class="dd-stickers">
					<text
						v-for="(item, i) in tabs"
						:key="i"
						class="dd-sticker"
						:class="['dd-sticker--' + i, { on: tab === i }]"
						@click="switchTab(i)"
					>{{ item }}</text>
				</view>

				<view class="dd-form">
					<!-- 密码登录 -->
					<block v-if="tab === 0">
						<view class="dd-field">
							<text class="dd-label">手机号 / 邮箱</text>
							<view class="dd-control" :class="{ 'is-focus': focus === 'account', 'is-error': !!err.account }">
								<input
									class="dd-input"
									v-model="account"
									:placeholder-style="phStyle"
									placeholder="写在这里…"
									maxlength="40"
									@focus="focus = 'account'"
									@blur="focus = ''"
								/>
							</view>
							<text v-if="err.account" class="dd-err">{{ err.account }}</text>
						</view>

						<view class="dd-field">
							<text class="dd-label">登录密码</text>
							<view class="dd-control" :class="{ 'is-focus': focus === 'password', 'is-error': !!err.password }">
								<input
									class="dd-input"
									v-model="password"
									:password="!showPwd"
									:placeholder-style="phStyle"
									placeholder="悄悄写…"
									maxlength="20"
									@focus="focus = 'password'"
									@blur="focus = ''"
								/>
								<text class="dd-chip" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '藏起来' : '看看' }}</text>
							</view>
							<text v-if="err.password" class="dd-err">{{ err.password }}</text>
						</view>
					</block>

					<!-- 手机验证码 -->
					<block v-else-if="tab === 1">
						<view class="dd-field">
							<text class="dd-label">手机号</text>
							<view class="dd-control" :class="{ 'is-focus': focus === 'phone', 'is-error': !!err.phone }">
								<input
									class="dd-input dd-input--num"
									v-model="phone"
									type="number"
									:placeholder-style="phStyle"
									placeholder="11 位手机号"
									maxlength="11"
									@focus="focus = 'phone'"
									@blur="focus = ''"
								/>
							</view>
							<text v-if="err.phone" class="dd-err">{{ err.phone }}</text>
						</view>

						<view class="dd-field">
							<text class="dd-label">短信验证码</text>
							<view class="dd-control" :class="{ 'is-focus': focus === 'sms', 'is-error': !!err.sms }">
								<input
									class="dd-input dd-input--num"
									v-model="sms"
									type="number"
									:placeholder-style="phStyle"
									placeholder="6 位数字"
									maxlength="6"
									@focus="focus = 'sms'"
									@blur="focus = ''"
								/>
								<text class="dd-chip" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + ' s' : '发一张' }}</text>
							</view>
							<text v-if="err.sms" class="dd-err">{{ err.sms }}</text>
						</view>
					</block>

					<!-- 邮箱验证码 -->
					<block v-else>
						<view class="dd-field">
							<text class="dd-label">邮箱地址</text>
							<view class="dd-control" :class="{ 'is-focus': focus === 'email', 'is-error': !!err.email }">
								<input
									class="dd-input"
									v-model="email"
									:placeholder-style="phStyle"
									placeholder="name@example.com"
									maxlength="40"
									@focus="focus = 'email'"
									@blur="focus = ''"
								/>
							</view>
							<text v-if="err.email" class="dd-err">{{ err.email }}</text>
						</view>

						<view class="dd-field">
							<text class="dd-label">邮箱验证码</text>
							<view class="dd-control" :class="{ 'is-focus': focus === 'mail', 'is-error': !!err.mail }">
								<input
									class="dd-input dd-input--num"
									v-model="mail"
									type="number"
									:placeholder-style="phStyle"
									placeholder="6 位数字"
									maxlength="6"
									@focus="focus = 'mail'"
									@blur="focus = ''"
								/>
								<text class="dd-chip" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + ' s' : '发一张' }}</text>
							</view>
							<text v-if="err.mail" class="dd-err">{{ err.mail }}</text>
						</view>
					</block>

					<view class="dd-row">
						<view class="dd-check dd-check--inline" @click="remember = !remember">
							<view class="dd-check__circle" :class="{ on: remember }"><view class="dd-check__tick"></view></view>
							<text class="dd-check__txt">记住这个本子</text>
						</view>
						<text class="dd-link dd-link--soft" @click="goForget">密码忘了？</text>
					</view>

					<!-- 印章按钮：按下像盖章 -->
					<view class="dd-stamp" :class="{ 'is-busy': submitting }" @click="submit">盖 章 登 录</view>

					<view class="dd-check" @click="agreed = !agreed">
						<view class="dd-check__circle" :class="{ on: agreed }"><view class="dd-check__tick"></view></view>
						<text class="dd-check__txt">我读过 <text class="dd-check__link">《用户协议》</text> 和 <text class="dd-check__link">《隐私政策》</text> 啦</text>
					</view>

					<view class="dd-doodle-alt">
						<text class="dd-doodle-alt__hint">～ 也可以这样进来 ～</text>
						<view class="dd-doodle-alt__row">
							<view class="dd-doodle-alt__i dd-doodle-alt__i--a" @click="toast('微信登录未接入')">微信</view>
							<view class="dd-doodle-alt__i dd-doodle-alt__i--b" @click="toast('企业微信登录未接入')">企微</view>
							<view class="dd-doodle-alt__i dd-doodle-alt__i--c" @click="toast('单点登录未接入')">SSO</view>
						</view>
					</view>
				</view>
			</view>
		</view>

		<view class="dd-foot">
			<view class="dd-foot__wave"></view>
			<view class="dd-foot__row">
				<text class="dd-foot__txt">还没有本子？</text>
				<text class="dd-link" @click="goRegister">新画一本 →</text>
			</view>
			<text class="dd-copy">© 2026 DOODLE NOTEBOOK · 画的</text>
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
				phStyle: 'color:#B9AF9E;font-size:15px;',
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
				this.account = 'momo@doodle.jp';
				this.password = 'scribble-2026';
				this.phone = '13800001234';
				this.sms = '202610';
				this.email = 'momo@doodle.jp';
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
					if (!this.isPhone(this.phone)) return this.toast('先写对手机号呀');
					this.smsLeft = 60;
				} else {
					if (this.mailLeft > 0) return;
					if (!this.isEmail(this.email)) return this.toast('先写对邮箱呀');
					this.mailLeft = 60;
				}
				// TODO: 接入后端验证码接口
				this.toast('验证码寄出啦');
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
					if (!this.account) e.account = '这里还没写呢';
					else if (this.account.indexOf('@') > -1 && !this.isEmail(this.account)) e.account = '邮箱好像画歪了';
					else if (this.account.indexOf('@') === -1 && !this.isPhone(this.account)) e.account = '手机号好像画歪了';
					if (!this.password) e.password = '这里还没写呢';
					else if (this.password.length < 6) e.password = '密码至少 6 位哦';
				} else if (this.tab === 1) {
					if (!this.isPhone(this.phone)) e.phone = '手机号好像画歪了';
					if (!this.sms) e.sms = '这里还没写呢';
				} else {
					if (!this.isEmail(this.email)) e.email = '邮箱好像画歪了';
					if (!this.mail) e.mail = '这里还没写呢';
				}
				this.err = e;
				return Object.keys(e).length === 0;
			},

			submit() {
				if (this.submitting) return;
				if (!this.agreed) return this.toast('先同意用户协议嘛');
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
				uni.navigateTo({ url: '/pages/login121/register' });
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login121/forget' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
