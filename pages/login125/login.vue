<template>
	<view class="in-page">
		<view class="in-status" :style="{ height: statusBar + 'px' }"></view>

		<view class="in-nav">
			<view class="in-back" @click="goBack">
				<view class="in-back__line"></view>
				<view class="in-back__tip"></view>
				<view class="in-back__tip in-back__tip--2"></view>
			</view>
			<text class="in-nav__idx">01 / 03</text>
		</view>

		<!-- 设备铭牌 -->
		<view class="in-plate">
			<view class="in-rivet in-rivet--tl"></view>
			<view class="in-rivet in-rivet--tr"></view>
			<view class="in-rivet in-rivet--bl"></view>
			<view class="in-rivet in-rivet--br"></view>
			<text class="in-plate__code">HEAVY DUTY ACCESS · TYPE-07</text>
			<text class="in-plate__title">控 制 台 登 录</text>
			<text class="in-plate__sub">操作前请确认回路连接正常，输入凭证以启动机组。</text>
		</view>
		<view class="in-hazard in-hazard--thin"></view>

		<!-- 控制柜 -->
		<view class="in-cabinet">
			<!-- 状态灯列 -->
			<view class="in-lamps">
				<view class="in-lamp">
					<view class="in-lamp__bulb in-lamp__bulb--pwr on"></view>
					<text class="in-lamp__txt">PWR</text>
				</view>
				<view class="in-lamp">
					<view class="in-lamp__bulb in-lamp__bulb--act" :class="{ on: submitting }"></view>
					<text class="in-lamp__txt">ACT</text>
				</view>
				<view class="in-lamp">
					<view class="in-lamp__bulb in-lamp__bulb--err" :class="{ on: hasErr() }"></view>
					<text class="in-lamp__txt">ERR</text>
				</view>
			</view>

			<!-- 档位开关：登录方式 -->
			<view class="in-gear">
				<view class="in-gear__knob" :style="{ transform: 'translateX(' + tab * 100 + '%)' }"></view>
				<text v-for="(item, i) in tabs" :key="i" class="in-gear__pos" :class="{ on: tab === i }" @click="switchTab(i)">{{ item }}</text>
			</view>

			<view class="in-form">
				<!-- 密码登录 -->
				<block v-if="tab === 0">
					<view class="in-field">
						<view class="in-field__bar">
							<text class="in-label">OPERATOR ID / 手机号 或 邮箱</text>
							<text class="in-code">IN-01</text>
						</view>
						<view class="in-slot" :class="{ 'is-focus': focus === 'account', 'is-error': !!err.account }">
							<input
								class="in-input"
								v-model="account"
								:placeholder-style="phStyle"
								placeholder="operator@plant.cn"
								maxlength="40"
								@focus="focus = 'account'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.account" class="in-err">{{ err.account }}</text>
					</view>

					<view class="in-field">
						<view class="in-field__bar">
							<text class="in-label">ACCESS KEY / 登录密码</text>
							<text class="in-code">IN-02</text>
						</view>
						<view class="in-slot" :class="{ 'is-focus': focus === 'password', 'is-error': !!err.password }">
							<input
								class="in-input"
								v-model="password"
								:password="!showPwd"
								:placeholder-style="phStyle"
								placeholder="••••••••"
								maxlength="20"
								@focus="focus = 'password'"
								@blur="focus = ''"
							/>
							<text class="in-suffix" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
						</view>
						<text v-if="err.password" class="in-err">{{ err.password }}</text>
					</view>
				</block>

				<!-- 手机验证码 -->
				<block v-else-if="tab === 1">
					<view class="in-field">
						<view class="in-field__bar">
							<text class="in-label">TERMINAL / 手机号</text>
							<text class="in-code">IN-01</text>
						</view>
						<view class="in-slot" :class="{ 'is-focus': focus === 'phone', 'is-error': !!err.phone }">
							<input
								class="in-input in-input--num"
								v-model="phone"
								type="number"
								:placeholder-style="phStyle"
								placeholder="11 位手机号"
								maxlength="11"
								@focus="focus = 'phone'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.phone" class="in-err">{{ err.phone }}</text>
					</view>

					<view class="in-field">
						<view class="in-field__bar">
							<text class="in-label">RELAY CODE / 短信验证码</text>
							<text class="in-code">IN-02</text>
						</view>
						<view class="in-slot" :class="{ 'is-focus': focus === 'sms', 'is-error': !!err.sms }">
							<input
								class="in-input in-input--num"
								v-model="sms"
								type="number"
								:placeholder-style="phStyle"
								placeholder="6 位数字"
								maxlength="6"
								@focus="focus = 'sms'"
								@blur="focus = ''"
							/>
							<text class="in-suffix in-suffix--code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
						</view>
						<text v-if="err.sms" class="in-err">{{ err.sms }}</text>
					</view>
				</block>

				<!-- 邮箱验证码 -->
				<block v-else>
					<view class="in-field">
						<view class="in-field__bar">
							<text class="in-label">MAILBOX / 邮箱地址</text>
							<text class="in-code">IN-01</text>
						</view>
						<view class="in-slot" :class="{ 'is-focus': focus === 'email', 'is-error': !!err.email }">
							<input
								class="in-input"
								v-model="email"
								:placeholder-style="phStyle"
								placeholder="operator@plant.cn"
								maxlength="40"
								@focus="focus = 'email'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.email" class="in-err">{{ err.email }}</text>
					</view>

					<view class="in-field">
						<view class="in-field__bar">
							<text class="in-label">RELAY CODE / 邮箱验证码</text>
							<text class="in-code">IN-02</text>
						</view>
						<view class="in-slot" :class="{ 'is-focus': focus === 'mail', 'is-error': !!err.mail }">
							<input
								class="in-input in-input--num"
								v-model="mail"
								type="number"
								:placeholder-style="phStyle"
								placeholder="6 位数字"
								maxlength="6"
								@focus="focus = 'mail'"
								@blur="focus = ''"
							/>
							<text class="in-suffix in-suffix--code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + ' s' : '获取验证码' }}</text>
						</view>
						<text v-if="err.mail" class="in-err">{{ err.mail }}</text>
					</view>
				</block>

				<view class="in-row">
					<view class="in-check in-check--inline" @click="remember = !remember">
						<view class="in-check__box" :class="{ on: remember }"><view class="in-check__tick"></view></view>
						<text class="in-check__txt">锁定本操作台</text>
					</view>
					<text class="in-link in-link--soft" @click="goForget">密钥遗失？</text>
				</view>

				<!-- 急停式主按钮 -->
				<view class="in-eStop" :class="{ 'is-busy': submitting }" @click="submit">启 动 · 登 录</view>

				<view class="in-check" @click="agreed = !agreed">
					<view class="in-check__box" :class="{ on: agreed }"><view class="in-check__tick"></view></view>
					<text class="in-check__txt">我已阅读并同意 <text class="in-check__link">《用户协议》</text> 与 <text class="in-check__link">《隐私政策》</text></text>
				</view>

				<!-- 刻度尺装饰 -->
				<view class="in-ruler"></view>

				<view class="in-alt">
					<view class="in-alt__i" @click="toast('微信登录未接入')">微信</view>
					<view class="in-alt__i" @click="toast('企业微信登录未接入')">企业微信</view>
					<view class="in-alt__i" @click="toast('单点登录未接入')">SSO</view>
				</view>
			</view>
		</view>

		<view class="in-hazard in-hazard--band"></view>

		<view class="in-foot">
			<view class="in-foot__row">
				<text class="in-foot__txt">还没有操作员账号</text>
				<text class="in-link" @click="goRegister">申请上岗证 →</text>
			</view>
			<text class="in-copy">© 2026 HEAVY DUTY ACCESS · SPEC-07 REV.C</text>
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
				phStyle: 'color:#5E646B;font-size:15px;',
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
				this.account = 'operator@plant.cn';
				this.password = 'torque-7708';
				this.phone = '13800001234';
				this.sms = '202610';
				this.email = 'operator@plant.cn';
				this.mail = '202610';
			},

			hasErr() {
				return Object.keys(this.err).length > 0;
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
				uni.navigateTo({ url: '/pages/login125/register' });
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login125/forget' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
