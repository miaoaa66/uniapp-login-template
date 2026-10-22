<template>
	<view class="sw-page">
		<view class="sw-grid"></view>
		<view class="sw-status" :style="{ height: statusBar + 'px' }"></view>

		<view class="sw-top">
			<view class="sw-back" @click="goBack">
				<view class="sw-back__arrow"></view>
			</view>
			<text class="sw-top__meta">SWISS GRID SYSTEM · 12 COL / CH</text>
			<text class="sw-top__idx">04—126</text>
		</view>
		<view class="sw-rule"></view>

		<view class="sw-hero">
			<view class="sw-hero__accent"></view>
			<view class="sw-hero__head">
				<text class="sw-hero__num">04</text>
				<view class="sw-hero__meta">
					<text class="sw-hero__kicker">CREDENTIAL RECOVERY</text>
					<text class="sw-hero__kicker sw-hero__kicker--dim">重置密码 · ZURÜCKSETZEN</text>
				</view>
			</view>
			<text class="sw-hero__title">找回密码</text>
			<text class="sw-hero__en">RECOVER / PASSWORT NEU</text>
			<text class="sw-hero__desc">先验证注册手机号，再设置新密码。流程同样按栏目编号排列。</text>
		</view>

		<view class="sw-steps">
			<view class="sw-step" :class="{ on: step === 0 }">
				<text class="sw-step__num">01</text>
				<text class="sw-step__txt">验证身份</text>
			</view>
			<view class="sw-step" :class="{ on: step === 1 }">
				<text class="sw-step__num">02</text>
				<text class="sw-step__txt">设置新密码</text>
			</view>
		</view>

		<view class="sw-form">
			<!-- 第一步：验证身份 -->
			<block v-if="step === 0">
				<view
					class="sw-field"
					:class="{ 'is-focus': focus === 'phone', 'is-on': focus === 'phone' || !!phone, 'is-error': !!err.phone }"
				>
					<text class="sw-field__label">注册手机号<text class="sw-field__req">*</text> / MOBILE</text>
					<text class="sw-field__idx">01</text>
					<input
						class="sw-field__input"
						v-model="phone"
						type="number"
						:placeholder-style="phStyle"
						placeholder=""
						maxlength="11"
						@focus="focus = 'phone'"
						@blur="focus = ''"
					/>
					<view class="sw-field__line"><view class="sw-field__ink"></view></view>
				</view>
				<text v-if="err.phone" class="sw-err">{{ err.phone }}</text>

				<view
					class="sw-field sw-field--code"
					:class="{ 'is-focus': focus === 'sms', 'is-on': focus === 'sms' || !!sms, 'is-error': !!err.sms }"
				>
					<text class="sw-field__label">验证码<text class="sw-field__req">*</text> / SMS CODE</text>
					<text class="sw-field__idx">02</text>
					<input
						class="sw-field__input"
						v-model="sms"
						type="number"
						:placeholder-style="phStyle"
						placeholder=""
						maxlength="6"
						@focus="focus = 'sms'"
						@blur="focus = ''"
					/>
					<text class="sw-field__code" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' s' : '获取验证码' }}</text>
					<view class="sw-field__line"><view class="sw-field__ink"></view></view>
				</view>
				<text v-if="err.sms" class="sw-err">{{ err.sms }}</text>

				<view class="sw-submit" @click="next">
					<text class="sw-submit__txt">下 一 步</text>
					<view class="sw-submit__right">
						<text class="sw-submit__en">NEXT</text>
						<text class="sw-submit__arrow">→</text>
					</view>
				</view>
			</block>

			<!-- 第二步：设置新密码 -->
			<block v-else>
				<view
					class="sw-field sw-field--toggle"
					:class="{ 'is-focus': focus === 'password', 'is-on': focus === 'password' || !!password, 'is-error': !!err.password }"
				>
					<text class="sw-field__label">新密码<text class="sw-field__req">*</text> / NEW PASSWORD</text>
					<text class="sw-field__idx">03</text>
					<input
						class="sw-field__input"
						v-model="password"
						:password="!showPwd"
						:placeholder-style="phStyle"
						placeholder=""
						maxlength="20"
						@focus="focus = 'password'"
						@blur="focus = ''"
					/>
					<text class="sw-field__toggle" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
					<view class="sw-field__line"><view class="sw-field__ink"></view></view>
				</view>
				<text v-if="err.password" class="sw-err">{{ err.password }}</text>
				<text v-else class="sw-hint">至少 8 位，同时包含字母与数字</text>

				<view
					class="sw-field sw-field--toggle"
					:class="{ 'is-focus': focus === 'confirm', 'is-on': focus === 'confirm' || !!confirm, 'is-error': !!err.confirm }"
				>
					<text class="sw-field__label">确认新密码<text class="sw-field__req">*</text> / CONFIRM</text>
					<text class="sw-field__idx">04</text>
					<input
						class="sw-field__input"
						v-model="confirm"
						:password="!showPwd"
						:placeholder-style="phStyle"
						placeholder=""
						maxlength="20"
						@focus="focus = 'confirm'"
						@blur="focus = ''"
					/>
					<view class="sw-field__line"><view class="sw-field__ink"></view></view>
				</view>
				<text v-if="err.confirm" class="sw-err">{{ err.confirm }}</text>

				<view class="sw-submit" :class="{ 'is-busy': submitting }" @click="submit">
					<text class="sw-submit__txt">重 置 密 码</text>
					<view class="sw-submit__right">
						<text class="sw-submit__en">RESET</text>
						<text class="sw-submit__arrow">→</text>
					</view>
				</view>
				<view class="sw-submit sw-submit--ghost" @click="back">
					<text class="sw-submit__txt">返回上一步</text>
				</view>
			</block>

			<view class="sw-note">
				<text class="sw-note__h">NOTE / 说明</text>
				<text class="sw-note__txt">重置成功后，其他设备上的登录状态将全部失效。若手机号已停用，请携带证件联系管理员核验。</text>
			</view>
		</view>

		<view class="sw-foot">
			<view class="sw-foot__rule"></view>
			<view class="sw-foot__body">
				<text class="sw-foot__txt">想起密码了？<text class="sw-link" @click="goLogin">返回登录</text></text>
				<text class="sw-foot__meta">SWISS · 02/20</text>
			</view>
			<view class="sw-foot__bar">
				<text>© 2026 GRID SYSTEM</text>
				<text>750 / 12 COL</text>
			</view>
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
				phStyle: 'color:#A7A39B;font-size:30rpx;',
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
				this.sms = '126126';
			}
		},
		onUnload() {
			this.clearTimer();
		},
		methods: {
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
				else uni.reLaunch({ url: '/pages/login126/login' });
			},

			goLogin() {
				uni.reLaunch({ url: '/pages/login126/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
