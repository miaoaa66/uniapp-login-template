<template>
	<view class="page">
		<view class="deco deco-wash"></view>
		<view class="deco deco-gold-v"></view>

		<view class="status" :style="{ height: statusBar + 'px' }"></view>

		<view class="nav">
			<view class="back" @click="goBack">
				<view class="back-line"></view>
				<view class="back-tip"></view>
				<view class="back-tip back-tip-2"></view>
			</view>
			<text class="nav-idx">叁 / 叁</text>
		</view>

		<view class="content">
			<view class="titlewrap">
				<view class="seal"><text class="seal-txt">补契</text></view>
				<view class="vtitle">
					<text class="vchar">重</text>
					<text class="vchar">立</text>
					<text class="vchar">口</text>
					<text class="vchar">令</text>
				</view>
			</view>

			<view class="head">
				<text class="lead">遗失口令勿忧。先核名帖，再立新契。</text>
			</view>

			<view class="divider"><view class="divider-ink"></view></view>

			<view class="steps">
				<view class="stp" :class="{ on: step === 0, done: step > 0 }">
					<text class="stp-dot"></text>
					<text class="stp-name">一验名帖</text>
				</view>
				<view class="stp-line"></view>
				<view class="stp" :class="{ on: step === 1 }">
					<text class="stp-dot stp-dot-red"></text>
					<text class="stp-name">二立新契</text>
				</view>
			</view>

			<view class="fields">
				<block v-if="step === 0">
					<view class="ifield" :class="{ active: focus === 'phone', error: !!err.phone }">
						<text class="ifield-label">名帖</text>
						<view class="ifield-main">
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
							<view class="ifield-inkline"></view>
						</view>
					</view>
					<text v-if="err.phone" class="err">{{ err.phone }}</text>

					<view class="ifield" :class="{ active: focus === 'sms', error: !!err.sms }">
						<text class="ifield-label">符验</text>
						<view class="ifield-main">
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
							<view class="ifield-inkline"></view>
						</view>
						<text class="code" :class="{ off: smsLeft > 0 }" @click="sendCode">{{ smsLeft > 0 ? smsLeft + ' 秒' : '取符' }}</text>
					</view>
					<text v-if="err.sms" class="err">{{ err.sms }}</text>

					<view class="stampbtn" @click="next">
						<text class="stampbtn-txt">核 名 过 帖</text>
					</view>
				</block>

				<block v-else>
					<view class="ifield" :class="{ active: focus === 'password', error: !!err.password }">
						<text class="ifield-label">新口令</text>
						<view class="ifield-main">
							<input
								class="input"
								v-model="password"
								:password="!showPwd"
								:placeholder-style="phStyle"
								placeholder="至少 8 位，含字母与数字"
								maxlength="20"
								@focus="focus = 'password'"
								@blur="focus = ''"
							/>
							<view class="ifield-inkline"></view>
						</view>
						<text class="eye" :class="{ on: showPwd }" @click="showPwd = !showPwd">{{ showPwd ? '隐' : '显' }}</text>
					</view>
					<text v-if="err.password" class="err">{{ err.password }}</text>

					<view class="ifield" :class="{ active: focus === 'confirm', error: !!err.confirm }">
						<text class="ifield-label">复核</text>
						<view class="ifield-main">
							<input
								class="input"
								v-model="confirm"
								:password="!showPwd"
								:placeholder-style="phStyle"
								placeholder="再次输入新口令"
								maxlength="20"
								@focus="focus = 'confirm'"
								@blur="focus = ''"
							/>
							<view class="ifield-inkline"></view>
						</view>
					</view>
					<text v-if="err.confirm" class="err">{{ err.confirm }}</text>

					<view class="stampbtn" :class="{ 'is-busy': submitting }" @click="submit">
						<text class="stampbtn-txt">{{ submitting ? '用印中…' : '钤 印 立 契' }}</text>
					</view>
					<view class="back-btn" @click="back">← 回到前一步</view>
				</block>

				<view class="safe">
					<text class="safe-h">※ 契约提示</text>
					<text class="safe-txt">重置成功后，其他设备上的登录状态将全部失效。若手机号已停用，请联系管理员核验身份。</text>
				</view>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text class="foot-txt">想起口令了</text>
				<text class="link" @click="goLogin">返山门</text>
			</view>
			<text class="copy">山静似太古 · 日长如小年</text>
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
				phStyle: 'color:rgba(31,31,31,0.32);font-size:15px;',
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
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号');
				this.smsLeft = 60;
				// TODO: 接入后端验证码接口
				this.toast('验证码已发送');
				if (this.timer) return;
				this.timer = setInterval(() => {
					if (this.smsLeft > 0) this.smsLeft--;
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
				else uni.reLaunch({ url: '/pages/login120/login' });
			},

			goLogin() {
				uni.reLaunch({ url: '/pages/login120/login' });
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
