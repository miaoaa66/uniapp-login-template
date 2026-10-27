<template>
	<view class="page">
		<view class="status" :style="{ height: statusBar + 'px' }"></view>

		<view class="nav">
			<view class="back" @click="goBack"><view class="back-line"></view></view>
			<text class="nav-idx">SEC 03 / 05</text>
		</view>

		<view class="radar">
			<view class="radar-line-h"></view>
			<view class="radar-line-v"></view>
			<view class="radar-sweep"></view>
			<view class="radar-core">
				<text class="radar-v">RESET</text>
				<text class="radar-h">KEY RECOVERY</text>
			</view>
		</view>

		<view class="head">
			<text class="title">重设口令</text>
			<text class="lead">核对你的身份后，即可重新设定访问口令。</text>
		</view>

		<view class="slots">
			<view class="slot" :class="{ active: focus === 'account', error: !!err.account }">
				<text class="slot-tag">ACC</text>
				<view class="slot-main">
					<text class="slot-label">账号</text>
					<input class="input" v-model="account" :placeholder-style="phStyle" placeholder="注册时的手机号 / 邮箱" maxlength="40"
						@focus="focus = 'account'" @blur="focus = ''" />
				</view>
			</view>
			<text v-if="err.account" class="err">{{ err.account }}</text>

			<view class="slot" :class="{ active: focus === 'code', error: !!err.code }">
				<text class="slot-tag">OTP</text>
				<view class="slot-main">
					<text class="slot-label">验证码</text>
					<input class="input input-num" v-model="code" type="number" :placeholder-style="phStyle" placeholder="6 位数字" maxlength="6"
						@focus="focus = 'code'" @blur="focus = ''" />
					<text class="code" :class="{ off: countdown > 0 }" @click="sendCode">{{ countdown > 0 ? countdown + ' s' : 'SEND' }}</text>
				</view>
			</view>
			<text v-if="err.code" class="err">{{ err.code }}</text>

			<view class="slot" :class="{ active: focus === 'password', error: !!err.password }">
				<text class="slot-tag">KEY</text>
				<view class="slot-main">
					<text class="slot-label">新口令</text>
					<input class="input" v-model="password" :password="!showPwd" :placeholder-style="phStyle" placeholder="8-20 位" maxlength="20"
						@focus="focus = 'password'" @blur="focus = ''" />
					<text class="eye" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
				</view>
			</view>
			<text v-if="err.password" class="err">{{ err.password }}</text>

			<view class="btn" @click="submit">重置口令</view>

			<view class="check" style="margin-top: 24rpx;" @click="agreed = !agreed">
				<view class="check-box" :class="{ on: agreed }"><view class="check-tick"></view></view>
				<text class="check-txt">我已阅读并同意 <text class="link">《用户协议》</text> 与 <text class="link">《隐私政策》</text></text>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text>想起口令了</text>
				<text class="link" @click="goBack">返回登录</text>
			</view>
			<text class="copy">© 2026 HOLO DECK · CLEARANCE L2</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				statusBar: 0,
				focus: '',
				showPwd: false,
				agreed: false,
				phStyle: 'color:rgba(94,140,136,0.8);font-size:15px;',
				account: '',
				code: '',
				password: '',
				err: {},
				countdown: 0,
				timer: null
			}
		},
		onLoad() {
			const info = uni.getSystemInfoSync()
			this.statusBar = info.statusBarHeight || 0
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			toast(title) {
				uni.showToast({ title, icon: 'none' })
			},

			sendCode() {
				if (this.countdown > 0) return
				if (!this.account) return this.toast('请输入账号')
				this.countdown = 60
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发送')
				this.startTimer()
			},

			startTimer() {
				if (this.timer) return
				this.timer = setInterval(() => {
					this.countdown--
					if (this.countdown <= 0) this.clearTimer()
				}, 1000)
			},

			clearTimer() {
				if (this.timer) {
					clearInterval(this.timer)
					this.timer = null
				}
			},

			submit() {
				const e = {}
				if (!this.account) e.account = '请输入账号'
				if (!this.code) e.code = '请输入验证码'
				if (!this.password) e.password = '请输入新口令'
				else if (this.password.length < 6) e.password = '口令不少于 6 位'
				this.err = e
				if (Object.keys(e).length > 0) return
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端重置密码接口，成功后返回登录页
				this.toast('口令已重置')
			},

			goBack() {
				const pages = getCurrentPages()
				if (pages.length > 1) uni.navigateBack()
				else uni.reLaunch({ url: '/pages/index' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
