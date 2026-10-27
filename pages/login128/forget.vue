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
				<view class="head-title">回炉重制</view>
				<view class="head-sub">验证一下身份，重新捏一颗新密码软糖。</view>
			</view>

			<view class="card pop pop-3">
				<view class="field">
					<text class="field-label">手机号 / 邮箱</text>
					<view class="field-box" :class="{ active: focusField === 'account' }">
						<input class="input" v-model="account" placeholder="请输入注册时使用的账号" placeholder-class="ph" maxlength="40"
							@focus="focusField = 'account'" @blur="focusField = ''" />
					</view>
				</view>

				<view class="field">
					<text class="field-label">验证码</text>
					<view class="field-box" :class="{ active: focusField === 'code' }">
						<input class="input" v-model="code" type="number" placeholder="6 位验证码" placeholder-class="ph" maxlength="6"
							@focus="focusField = 'code'" @blur="focusField = ''" />
						<text class="code-btn" :class="{ dim: countdown > 0 }" @click="sendCode">
							{{ countdown > 0 ? countdown + 's' : '发送' }}
						</text>
					</view>
				</view>

				<view class="field">
					<text class="field-label">新密码</text>
					<view class="field-box" :class="{ active: focusField === 'password' }">
						<input class="input" v-model="password" :password="!showPassword" placeholder="8-20 位新密码" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password'" @blur="focusField = ''" />
						<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐藏' : '显示' }}</text>
					</view>
				</view>

				<view class="submit" @click="submit">
					<text class="submit-text">重新捏好</text>
				</view>

				<view class="foot-link">
					想起密码了？<text class="link" @click="goLogin">回去登录</text>
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
				account: '',
				code: '',
				password: '',
				showPassword: false,
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

			sendCode() {
				if (this.countdown > 0) return
				if (!this.account) return this.toast('请先输入账号')
				this.countdown = 60
				// TODO: 调用后端发送验证码接口（按账号类型走短信/邮件）
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
				if (!this.account) return this.toast('请输入账号')
				if (!this.code) return this.toast('请输入验证码')
				if (!this.password || this.password.length < 8) return this.toast('新密码不少于 8 位')
				// TODO: 调用后端重置密码接口，成功后返回登录页
				this.toast('重制完成 · 请重新登录')
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
