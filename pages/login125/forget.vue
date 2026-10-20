<template>
	<view class="in-page">
		<view class="in-status" :style="{ height: statusBar + 'px' }"></view>

		<view class="in-nav">
			<view class="in-back" @click="goBack">
				<view class="in-back__line"></view>
				<view class="in-back__tip"></view>
				<view class="in-back__tip in-back__tip--2"></view>
			</view>
			<text class="in-nav__idx">03 / 03</text>
		</view>

		<view class="in-plate">
			<view class="in-rivet in-rivet--tl"></view>
			<view class="in-rivet in-rivet--tr"></view>
			<view class="in-rivet in-rivet--bl"></view>
			<view class="in-rivet in-rivet--br"></view>
			<text class="in-plate__code">KEY RESET · MAINT-03</text>
			<text class="in-plate__title">密 钥 重 配</text>
			<text class="in-plate__sub">通过注册终端号重新校验操作员身份，随后配置新密钥。</text>
		</view>
		<view class="in-hazard in-hazard--thin"></view>

		<view class="in-cabinet">
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

			<!-- 检修工序：指示灯 + 导通管线 -->
			<view class="in-pipe">
				<view class="in-pipe__node" :class="{ on: step >= 0, done: step > 0 }">
					<text class="in-pipe__num">01</text>
				</view>
				<view class="in-pipe__wire" :class="{ live: step > 0 }"></view>
				<view class="in-pipe__node" :class="{ on: step > 0 }">
					<text class="in-pipe__num">02</text>
				</view>
			</view>
			<view class="in-pipe__legend">
				<text>校验身份</text>
				<text>配置密钥</text>
			</view>

			<view class="in-form">
				<!-- 第一步：校验身份 -->
				<block v-if="step === 0">
					<view class="in-field">
						<view class="in-field__bar">
							<text class="in-label">TERMINAL / 注册手机号</text>
							<text class="in-code">MT-01</text>
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
							<text class="in-code">MT-02</text>
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
							<text class="in-suffix in-suffix--code" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
						</view>
						<text v-if="err.sms" class="in-err">{{ err.sms }}</text>
					</view>

					<view class="in-eStop in-eStop--go" @click="next">进 入 下 一 工 序</view>
				</block>

				<!-- 第二步：配置密钥 -->
				<block v-else>
					<view class="in-field">
						<view class="in-field__bar">
							<text class="in-label">NEW KEY / 新密码</text>
							<text class="in-code">MT-03</text>
						</view>
						<view class="in-slot" :class="{ 'is-focus': focus === 'password', 'is-error': !!err.password }">
							<input
								class="in-input"
								v-model="password"
								:password="!showPwd"
								:placeholder-style="phStyle"
								placeholder="至少 8 位，含字母与数字"
								maxlength="20"
								@focus="focus = 'password'"
								@blur="focus = ''"
							/>
							<text class="in-suffix" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
						</view>
						<text v-if="err.password" class="in-err">{{ err.password }}</text>
					</view>

					<view class="in-field">
						<view class="in-field__bar">
							<text class="in-label">CONFIRM / 确认新密码</text>
							<text class="in-code">MT-04</text>
						</view>
						<view class="in-slot" :class="{ 'is-focus': focus === 'confirm', 'is-error': !!err.confirm }">
							<input
								class="in-input"
								v-model="confirm"
								:password="!showPwd"
								:placeholder-style="phStyle"
								placeholder="再次输入新密码"
								maxlength="20"
								@focus="focus = 'confirm'"
								@blur="focus = ''"
							/>
						</view>
						<text v-if="err.confirm" class="in-err">{{ err.confirm }}</text>
					</view>

					<view class="in-eStop" :class="{ 'is-busy': submitting }" @click="submit">确 认 · 重 配</view>
					<view class="in-eStop in-eStop--ghost" @click="back">返回上一步</view>
				</block>

				<view class="in-manual">
					<text class="in-manual__h">检修手册 · 3.2 节</text>
					<text class="in-manual__txt">重配成功后，其他设备的登录状态将全部失效。若终端号已停用，请携带工牌联系管理员现场核验。</text>
				</view>
			</view>
		</view>

		<view class="in-hazard in-hazard--band"></view>

		<view class="in-foot">
			<view class="in-foot__row">
				<text class="in-foot__txt">想起密钥了？</text>
				<text class="in-link" @click="goLogin">返回控制台 →</text>
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
				step: 0,
				focus: '',
				showPwd: false,
				submitting: false,
				phStyle: 'color:#5E646B;font-size:15px;',
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
			if (options && options.demo) {
				this.phone = '13800001234';
				this.sms = '202610';
			}
		},
		onUnload() {
			this.clearTimer();
		},
		methods: {
			hasErr() {
				return Object.keys(this.err).length > 0;
			},

			toast(title) {
				uni.showToast({ title, icon: 'none' });
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
					this.smsLeft--;
					if (this.smsLeft <= 0) this.clearTimer();
				}, 1000);
			},

			clearTimer() {
				if (this.timer) {
					clearInterval(this.timer);
					this.timer = null;
				}
			},

			next() {
				const e = {};
				if (!this.isPhone(this.phone)) e.phone = '请输入正确的手机号';
				if (!this.sms) e.sms = '请输入短信验证码';
				else if (this.sms.length !== 6) e.sms = '验证码为 6 位数字';
				this.err = e;
				if (Object.keys(e).length) return;
				// TODO: 校验验证码是否通过
				this.clearTimer();
				this.smsLeft = 0;
				this.err = {};
				this.step = 1;
			},

			back() {
				this.err = {};
				this.step = 0;
			},

			validate() {
				const e = {};
				if (!this.password) e.password = '请设置新密码';
				else if (this.password.length < 8) e.password = '密码不少于 8 位';
				else if (!/[a-zA-Z]/.test(this.password) || !/\d/.test(this.password)) e.password = '密码需同时包含字母与数字';
				if (!this.confirm) e.confirm = '请再次输入新密码';
				else if (this.confirm !== this.password) e.confirm = '两次输入的密码不一致';
				this.err = e;
				return Object.keys(e).length === 0;
			},

			submit() {
				if (this.submitting) return;
				if (!this.validate()) return;
				this.submitting = true;
				// TODO: 调用后端重置密码接口
				setTimeout(() => {
					this.submitting = false;
					this.toast('密码已重置');
				}, 600);
			},

			goBack() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/login125/login' });
			},

			goLogin() {
				uni.reLaunch({ url: '/pages/login125/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
