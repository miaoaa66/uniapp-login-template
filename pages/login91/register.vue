<template>
	<view class="page">
		<view class="brand">
			<view class="logo">
				<view class="logo-inner"></view>
			</view>
			<text class="brand-name">云汇通</text>
		</view>

		<view class="head">
			<view class="head-title">注册账号</view>
			<view class="head-sub">使用手机号快速创建新账号</view>
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

			<view class="field">
				<text class="field-label">密码</text>
				<view class="field-box">
					<input v-model="password" :password="!showPassword" placeholder="请设置登录密码（6-20位）" maxlength="20" />
					<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐藏' : '显示' }}</text>
				</view>
			</view>

			<view class="field">
				<text class="field-label">确认密码</text>
				<view class="field-box">
					<input v-model="confirmPassword" :password="!showPassword" placeholder="请再次输入密码" maxlength="20" />
				</view>
			</view>

			<view class="submit-btn" @click="submit">注 册</view>

			<view class="foot-link">
				已有账号？<text @click="goLogin">返回登录</text>
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
				showPassword: false,
				agreed: false,
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
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端注册接口，成功后返回登录页
				this.toast('注册成功')
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
