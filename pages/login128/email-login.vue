<template>
	<view class="page">
		<view class="blob blob-1"></view>
		<view class="blob blob-2"></view>

		<view class="content">
			<view class="tray pop pop-1">
				<view class="tray-disc">
					<view class="tray-jelly"></view>
				</view>
				<view class="tray-shadow"></view>
				<view class="tray-name">果冻工坊</view>
				<view class="tray-sub">JELLY TRAY</view>
			</view>

			<view class="head pop pop-2">
				<view class="head-title">邮箱开门</view>
				<view class="head-sub">验证码会寄到你的邮箱，请留意查收。</view>
			</view>

			<view class="chips pop pop-3">
				<view class="chip" @click="goPwd">密码</view>
				<view class="chip" @click="goPhone">短信</view>
				<view class="chip on">邮箱</view>
			</view>

			<view class="card pop pop-3">
				<view class="field">
					<text class="field-label">邮箱</text>
					<view class="field-box" :class="{ active: focusField === 'email' }">
						<input class="input" v-model="email" placeholder="请输入邮箱地址" placeholder-class="ph" maxlength="60"
							@focus="focusField = 'email'" @blur="focusField = ''" />
					</view>
				</view>

				<view class="field">
					<text class="field-label">邮箱验证码</text>
					<view class="field-box" :class="{ active: focusField === 'code' }">
						<input class="input" v-model="code" type="number" placeholder="6 位验证码" placeholder-class="ph" maxlength="6"
							@focus="focusField = 'code'" @blur="focusField = ''" />
						<text class="code-btn" :class="{ dim: countdown > 0 }" @click="sendCode">
							{{ countdown > 0 ? countdown + 's' : '发送' }}
						</text>
					</view>
				</view>

				<view class="submit" @click="submit">
					<text class="submit-text">验证登录</text>
				</view>

				<view class="agreement">
					<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
					<text>我已阅读并同意<text class="link">《用户协议》</text>与<text class="link">《隐私政策》</text></text>
				</view>
			</view>

			<view class="foot-note pop pop-4">© 2026 JELLY TRAY · 果冻工坊</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				email: '',
				code: '',
				agreed: false,
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

			isEmail(v) {
				return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v)
			},

			goPwd() {
				uni.navigateBack()
			},

			goPhone() {
				uni.navigateTo({
					url: '/pages/login128/phone-login'
				})
			},

			sendCode() {
				if (this.countdown > 0) return
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱地址')
				this.countdown = 60
				// TODO: 调用后端发送邮箱验证码接口
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
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱地址')
				if (!this.code) return this.toast('请输入邮箱验证码')
				// TODO: 调用后端验证码登录接口，成功后跳转首页
				this.toast('登录成功')
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
