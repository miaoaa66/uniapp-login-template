<template>
	<view class="page">
		<view class="blob blob-1"></view>
		<view class="blob blob-2"></view>

		<view class="content">
			<view class="studio pop pop-1">
				<view class="studio-logo"></view>
				<view>
					<view class="studio-name">黏土工坊</view>
					<view class="studio-sub">CLAY STUDIO</view>
				</view>
			</view>

			<view class="head pop pop-2">
				<view class="head-title">手机号登录</view>
				<view class="head-sub">未注册的手机号将自动为你创建一个账号。</view>
			</view>

			<view class="card pop pop-3">
				<view class="field">
					<text class="field-label">手机号</text>
					<view class="field-box" :class="{ active: focusField === 'phone' }">
						<text class="field-prefix">+86</text>
						<input class="input" v-model="phone" type="number" placeholder="请输入手机号" placeholder-class="ph" maxlength="11"
							@focus="focusField = 'phone'" @blur="focusField = ''" />
					</view>
				</view>

				<view class="field">
					<text class="field-label">短信验证码</text>
					<view class="field-box" :class="{ active: focusField === 'code' }">
						<input class="input" v-model="code" type="number" placeholder="请输入 6 位验证码" placeholder-class="ph" maxlength="6"
							@focus="focusField = 'code'" @blur="focusField = ''" />
						<text class="code-btn" :class="{ dim: countdown > 0 }" @click="sendCode">
							{{ countdown > 0 ? countdown + 's' : '获取验证码' }}
						</text>
					</view>
				</view>

				<view class="submit" @click="submit">
					<text class="submit-text">登 录</text>
				</view>

				<view class="agreement">
					<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
					<text>我已阅读并同意<text class="link">《用户协议》</text>与<text class="link">《隐私政策》</text></text>
				</view>
			</view>

			<view class="submit plain pop pop-4" @click="goEmailLogin">
				<text class="submit-text">邮箱验证码登录</text>
			</view>

			<view class="foot-note pop pop-4">© 2026 CLAY STUDIO · 黏土工坊</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				agreed: false,
				phone: '',
				code: '',
				focusField: '',
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
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
				if (!this.code) return this.toast('请输入短信验证码')
				// TODO: 调用后端验证码登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goLogin() {
				uni.navigateBack()
			},

			goEmailLogin() {
				uni.redirectTo({
					url: '/pages/login128/email-login'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
