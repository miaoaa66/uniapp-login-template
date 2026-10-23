<template>
	<view class="page">
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
			<view class="receipt">
				<view class="head">
					<text class="shop">A&middot;G MARKET</text>
					<text class="slogan">— 收银凭据 / CUSTOMER COPY —</text>
				</view>

				<view class="meta">
					<text class="meta-line">单号 NO.2026-1023-07</text>
					<text class="meta-line">机台 TERMINAL 03 // 收银员 07</text>
				</view>

				<view class="dash"></view>

				<view class="punches">
					<view v-for="(item, i) in tabs" :key="i" class="punch" :class="{ on: tab === i }" @click="switchTab(i)">
						<view class="punch-hole"></view>
						<text class="punch-name">{{ item }}登录</text>
					</view>
				</view>

				<view class="dash"></view>

				<view class="entries">
					<block v-if="tab === 0">
						<view class="entry" :class="{ active: focus === 'account', error: !!err.account }">
							<text class="entry-label">账号</text>
							<view class="entry-leader"></view>
							<input
								class="input input-num"
								v-model="account"
								:placeholder-style="phStyle"
								placeholder="手机号或邮箱"
								maxlength="40"
								@focus="focus = 'account'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.account" class="err">{{ err.account }}</text>

						<view class="entry" :class="{ active: focus === 'password', error: !!err.password }">
							<text class="entry-label">密码</text>
							<view class="entry-leader"></view>
							<input
								class="input"
								v-model="password"
								:password="!showPwd"
								:placeholder-style="phStyle"
								placeholder="登录密码"
								maxlength="20"
								@focus="focus = 'password'"
								@blur="focus = ''"
							/>
							<text class="eye" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐' : '显' }}</text>
						</view>
						<text v-if="err.password" class="err">{{ err.password }}</text>
					</block>

					<block v-else-if="tab === 1">
						<view class="entry" :class="{ active: focus === 'phone', error: !!err.phone }">
							<text class="entry-label">手机</text>
							<view class="entry-leader"></view>
							<input
								class="input input-num"
								v-model="phone"
								type="number"
								:placeholder-style="phStyle"
								placeholder="11 位手机号"
								maxlength="11"
								@focus="focus = 'phone'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.phone" class="err">{{ err.phone }}</text>

						<view class="entry" :class="{ active: focus === 'sms', error: !!err.sms }">
							<text class="entry-label">短码</text>
							<view class="entry-leader"></view>
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
							<text class="code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '取码' }}</text>
						</view>
						<text v-if="err.sms" class="err">{{ err.sms }}</text>
					</block>

					<block v-else>
						<view class="entry" :class="{ active: focus === 'email', error: !!err.email }">
							<text class="entry-label">邮箱</text>
							<view class="entry-leader"></view>
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
						<text v-if="err.email" class="err">{{ err.email }}</text>

						<view class="entry" :class="{ active: focus === 'mail', error: !!err.mail }">
							<text class="entry-label">邮码</text>
							<view class="entry-leader"></view>
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
							<text class="code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '取码' }}</text>
						</view>
						<text v-if="err.mail" class="err">{{ err.mail }}</text>
					</block>

					<view class="row">
						<view class="check check-inline" @click="remember = !remember">
							<view class="check-box" :class="{ on: remember }"><text v-if="remember" class="check-tick">●</text></view>
							<text class="check-txt">记住设备</text>
						</view>
						<text class="link link-soft" @click="goForget">挂失 / 找回</text>
					</view>

					<view class="tear" :class="{ 'is-busy': submitting }" @click="submit">
						<text class="tear-txt">{{ submitting ? '打印中…' : '✂ - - - 撕下此联 登录 - - -' }}</text>
					</view>

					<view class="check" @click="agreed = !agreed">
						<view class="check-box" :class="{ on: agreed }"><text v-if="agreed" class="check-tick">●</text></view>
						<text class="check-txt">我已阅读并同意 <text class="check-link">《用户协议》</text> 与 <text class="check-link">《隐私政策》</text></text>
					</view>
				</view>

				<view class="dash"></view>

				<view class="barcode">
					<view class="barcode-bars"></view>
					<text class="barcode-num">2 0 2 6 1 0 2 3 0 7</text>
				</view>

				<text class="thanks">THANK YOU // PLEASE COME AGAIN</text>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text class="foot-txt">还没有会员卡？</text>
				<text class="link" @click="goRegister">办理入会</text>
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
				phStyle: 'color:rgba(58,50,42,0.35);font-size:15px;',
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
				uni.navigateTo({ url: '/pages/login119/register' });
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login119/forget' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
