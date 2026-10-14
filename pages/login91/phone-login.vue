<template>
	<view class="page">
		<view class="brand">
			<view class="logo">
				<view class="logo-inner"></view>
			</view>
			<text class="brand-name">云汇通</text>
		</view>

		<view class="head">
			<view class="head-title">手机号登录</view>
			<view class="head-sub">未注册的手机号将自动创建账号</view>
		</view>

		<view class="card">
			<view class="field">
				<text class="field-label">手机号</text>
				<view class="field-box">
					<text class="field-prefix">+86</text>
					<input v-model="phone" type="number" placeholder="请输入手机号" maxlength="11" />
				</view>
			</view>

			<view class="field">
				<text class="field-label">验证码</text>
				<view class="field-box">
					<input v-model="smsCode" type="number" placeholder="请输入短信验证码" maxlength="6" />
					<text class="code-btn" :class="{ gray: countdown > 0 }" @click="sendCode">
						{{ countdown > 0 ? countdown + 's' : '获取验证码' }}
					</text>
				</view>
			</view>

			<view class="submit-btn" @click="submit">登 录</view>

			<view class="divider"><text>其他登录方式</text></view>

			<view class="other-row">
				<view class="submit-btn plain" @click="goEmail">邮箱验证码登录</view>
			</view>

			<view class="foot-link">
				<text @click="goLogin">使用账号密码登录</text>
			</view>

			<view class="agreement">
				<view class="checkbox" :class="{ checked: agreed }" @click="agreed = !agreed"></view>
				<text>我已阅读并同意<text class="link">《用户协议》</text>和<text class="link">《隐私政策》</text></text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				agreed: false,
				phone: '',
				smsCode: '',
				countdown: 0,
				timer: null
			}
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			isPhone(value) {
				return /^1\d{10}$/.test(value)
			},

			// 发送验证码
			sendCode() {
				if (this.countdown > 0) return
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
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
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
				if (!this.smsCode) return this.toast('请输入短信验证码')
				// TODO: 调用后端验证码登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goLogin() {
				uni.navigateBack()
			},

			goEmail() {
				uni.redirectTo({
					url: '/pages/login91/email-login'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
