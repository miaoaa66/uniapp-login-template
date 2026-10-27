<template>
	<view class="page">
		<view class="status" :style="{ height: statusBar + 'px' }"></view>

		<view class="nav">
			<view class="back" @click="goBack"><view class="back-line"></view></view>
			<text class="nav-idx">SEC 04 / 05</text>
		</view>

		<view class="radar">
			<view class="radar-line-h"></view>
			<view class="radar-line-v"></view>
			<view class="radar-sweep"></view>
			<view class="radar-core">
				<text class="radar-v">SMS</text>
				<text class="radar-h">TEL ACCESS</text>
			</view>
		</view>

		<view class="head">
			<text class="title">短信验证登录</text>
			<text class="lead">未登记的手机号将自动建立乘员档案。</text>
		</view>

		<view class="slots">
			<view class="slot" :class="{ active: focus === 'phone', error: !!err.phone }">
				<text class="slot-tag">TEL</text>
				<view class="slot-main">
					<text class="slot-label">手机号</text>
					<input class="input input-num" v-model="phone" type="number" :placeholder-style="phStyle" placeholder="11 位手机号" maxlength="11"
						@focus="focus = 'phone'" @blur="focus = ''" />
				</view>
			</view>
			<text v-if="err.phone" class="err">{{ err.phone }}</text>

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

			<view class="btn" @click="submit">验证登录</view>

			<view class="check" style="margin-top: 24rpx;" @click="agreed = !agreed">
				<view class="check-box" :class="{ on: agreed }"><view class="check-tick"></view></view>
				<text class="check-txt">我已阅读并同意 <text class="link">《用户协议》</text> 与 <text class="link">《隐私政策》</text></text>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text>其他方式</text>
				<text class="link" @click="goEmailLogin">邮箱登录</text>
				<text class="link" style="margin-left: 24rpx;" @click="goBack">密码登录</text>
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
				agreed: false,
				phStyle: 'color:rgba(94,140,136,0.8);font-size:15px;',
				phone: '',
				code: '',
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

			isPhone(v) {
				return /^1\d{10}$/.test(v)
			},

			sendCode() {
				if (this.countdown > 0) return
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
				this.countdown = 60
				// TODO: 调用后端发送短信验证码接口
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
				if (!this.isPhone(this.phone)) e.phone = '请输入正确的手机号'
				if (!this.code) e.code = '请输入短信验证码'
				this.err = e
				if (Object.keys(e).length > 0) return
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端验证码登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goBack() {
				const pages = getCurrentPages()
				if (pages.length > 1) uni.navigateBack()
				else uni.reLaunch({ url: '/pages/index' })
			},

			goEmailLogin() {
				uni.redirectTo({ url: '/pages/login129/email-login' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
