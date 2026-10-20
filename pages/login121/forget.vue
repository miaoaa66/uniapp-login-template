<template>
	<view class="dd-page">
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
			<text class="dd-nav__idx">03 / 03</text>
		</view>

		<view class="dd-main">
			<view class="dd-head">
				<text class="dd-eyebrow">OH NO · 钥匙丢了</text>
				<text class="dd-title">找回密码</text>
				<text class="dd-lead">别慌，用手机号验证一下，再画一把新钥匙。</text>
			</view>

			<!-- 步骤：手绘圆点 + 虚线小路 -->
			<view class="dd-steps">
				<view class="dd-step" :class="{ on: step === 0 }">
					<view class="dd-step__dot"><text v-if="step > 0" class="dd-step__done">✓</text></view>
					<text class="dd-step__txt">01 拿到暗号</text>
				</view>
				<view class="dd-step__path"></view>
				<view class="dd-step" :class="{ on: step === 1 }">
					<view class="dd-step__dot"><text v-if="step > 1" class="dd-step__done">✓</text></view>
					<text class="dd-step__txt">02 画新钥匙</text>
				</view>
			</view>

			<view class="dd-note">
				<view class="dd-note__tape dd-note__tape--l"></view>
				<view class="dd-note__tape dd-note__tape--r"></view>

				<view class="dd-form">
					<!-- 第一步：验证身份 -->
					<block v-if="step === 0">
						<view class="dd-field">
							<text class="dd-label">注册手机号</text>
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
								<text class="dd-chip" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' s' : '发一张' }}</text>
							</view>
							<text v-if="err.sms" class="dd-err">{{ err.sms }}</text>
						</view>

						<view class="dd-stamp" @click="next">下 一 页</view>
					</block>

					<!-- 第二步：设置新密码 -->
					<block v-else>
						<view class="dd-field">
							<text class="dd-label">新密码</text>
							<view class="dd-control" :class="{ 'is-focus': focus === 'password', 'is-error': !!err.password }">
								<input
									class="dd-input"
									v-model="password"
									:password="!showPwd"
									:placeholder-style="phStyle"
									placeholder="至少 8 位，字母加数字"
									maxlength="20"
									@focus="focus = 'password'"
									@blur="focus = ''"
								/>
								<text class="dd-chip" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '藏起来' : '看看' }}</text>
							</view>
							<text v-if="err.password" class="dd-err">{{ err.password }}</text>
						</view>

						<view class="dd-field">
							<text class="dd-label">确认新密码</text>
							<view class="dd-control" :class="{ 'is-focus': focus === 'confirm', 'is-error': !!err.confirm }">
								<input
									class="dd-input"
									v-model="confirm"
									:password="!showPwd"
									:placeholder-style="phStyle"
									placeholder="再写一遍"
									maxlength="20"
									@focus="focus = 'confirm'"
									@blur="focus = ''"
								/>
							</view>
							<text v-if="err.confirm" class="dd-err">{{ err.confirm }}</text>
						</view>

						<view class="dd-stamp" :class="{ 'is-busy': submitting }" @click="submit">盖 章 重 置</view>
						<view class="dd-stamp dd-stamp--ghost" @click="back">上一页</view>
					</block>

					<!-- 便利贴提示 -->
					<view class="dd-sticky">
						<view class="dd-sticky__tape"></view>
						<text class="dd-sticky__h">安全小贴纸</text>
						<text class="dd-sticky__txt">重置成功后，其他设备会被请下线哦。手机号停用了？找管理员画画押。</text>
					</view>
				</view>
			</view>
		</view>

		<view class="dd-foot">
			<view class="dd-foot__wave"></view>
			<view class="dd-foot__row">
				<text class="dd-foot__txt">想起来啦？</text>
				<text class="dd-link" @click="goLogin">回去登录 →</text>
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
				step: 0,
				focus: '',
				showPwd: false,
				submitting: false,
				phStyle: 'color:#B9AF9E;font-size:15px;',
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
			toast(title) {
				uni.showToast({ title, icon: 'none' });
			},

			isPhone(v) {
				return /^1\d{10}$/.test(v);
			},

			sendCode() {
				if (this.smsLeft > 0) return;
				if (!this.isPhone(this.phone)) return this.toast('先写对手机号呀');
				this.smsLeft = 60;
				// TODO: 接入后端验证码接口
				this.toast('验证码寄出啦');
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
				if (!this.isPhone(this.phone)) e.phone = '手机号好像画歪了';
				if (!this.sms) e.sms = '这里还没写呢';
				else if (this.sms.length !== 6) e.sms = '暗号是 6 位数字';
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
				if (!this.password) e.password = '这里还没写呢';
				else if (this.password.length < 8) e.password = '密码至少 8 位哦';
				else if (!/[a-zA-Z]/.test(this.password) || !/\d/.test(this.password)) e.password = '字母和数字都要有哦';
				if (!this.confirm) e.confirm = '再写一遍吧';
				else if (this.confirm !== this.password) e.confirm = '两次写得不一样';
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
					this.toast('新钥匙画好啦');
				}, 600);
			},

			goBack() {
				const pages = getCurrentPages();
				if (pages.length > 1) uni.navigateBack();
				else uni.reLaunch({ url: '/pages/login121/login' });
			},

			goLogin() {
				uni.reLaunch({ url: '/pages/login121/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
