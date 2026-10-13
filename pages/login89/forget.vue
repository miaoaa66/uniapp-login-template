<template>
	<view class="login-page">
		<view class="page-title">找回密码</view>
		<view class="page-subtitle">验证手机号后即可重置登录密码</view>

		<view class="form">
			<view class="field">
				<text class="field-label">+86</text>
				<input v-model="phone" type="number" placeholder="请输入注册手机号" maxlength="11" />
			</view>
			<view class="field">
				<text class="field-label">验证码</text>
				<input v-model="smsCode" type="number" placeholder="请输入短信验证码" maxlength="6" />
				<text class="code-btn" :class="{ gray: countdown > 0 }" @click="sendCode">
					{{ countdown > 0 ? countdown + 's' : '获取验证码' }}
				</text>
			</view>
			<view class="field">
				<text class="field-label">新密码</text>
				<input v-model="password" :password="!showPassword" placeholder="请设置新密码（6-20位）" maxlength="20" />
				<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐藏' : '显示' }}</text>
			</view>
			<view class="field">
				<text class="field-label">确认</text>
				<input v-model="confirmPassword" :password="!showPassword" placeholder="请再次输入新密码" maxlength="20" />
			</view>

			<view class="submit-btn" @click="submit">重置密码</view>

			<view class="foot-link">
				<text @click="goLogin">返回登录</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showPassword: false,
				phone: '',
				smsCode: '',
				password: '',
				confirmPassword: '',
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
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
				if (!this.smsCode) return this.toast('请输入短信验证码')
				if (!this.password || this.password.length < 6) return this.toast('密码长度不少于6位')
				if (this.password !== this.confirmPassword) return this.toast('两次输入的密码不一致')
				// TODO: 调用后端重置密码接口，成功后返回登录页
				this.toast('密码重置成功')
			},

			goLogin() {
				uni.navigateBack()
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>