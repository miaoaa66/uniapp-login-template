<template>
	<view class="page">
		<view class="aurora aurora-1"></view>
		<view class="aurora aurora-2"></view>
		<view class="aurora aurora-3"></view>

		<view class="content">
			<view class="brand">
				<view class="logo">
					<view class="logo-core"></view>
				</view>
				<text class="brand-name">LUMEN</text>
				<text class="brand-sub">AURORA ACCOUNT</text>
			</view>

			<view class="head">
				<view class="head-title">重置密码</view>
				<view class="head-sub">验证你绑定的账号，即可设置一个新的登录密码。</view>
			</view>

			<view class="card">
				<view class="field">
					<text class="field-label">手机号 / 邮箱</text>
					<view class="field-box" :class="{ active: focusField === 'contact' }">
						<input class="input" v-model="contact" placeholder="请输入绑定账号" placeholder-class="ph" maxlength="40"
							@focus="focusField = 'contact'" @blur="focusField = ''" />
					</view>
				</view>

				<view class="field">
					<text class="field-label">验证码</text>
					<view class="field-box" :class="{ active: focusField === 'code' }">
						<input class="input" v-model="code" placeholder="请输入 6 位验证码" placeholder-class="ph" maxlength="6"
							@focus="focusField = 'code'" @blur="focusField = ''" />
						<text class="code-btn" :class="{ dim: counting }" @click="sendCode">{{ counting ? seconds + 's' : '获取验证码' }}</text>
					</view>
				</view>

				<view class="field">
					<text class="field-label">新密码</text>
					<view class="field-box" :class="{ active: focusField === 'password' }">
						<input class="input" v-model="password" :password="!showPassword" placeholder="8-20 位，建议含字母与数字" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password'" @blur="focusField = ''" />
						<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐藏' : '显示' }}</text>
					</view>
				</view>

				<view class="field">
					<text class="field-label">确认新密码</text>
					<view class="field-box" :class="{ active: focusField === 'password2' }">
						<input class="input" v-model="password2" :password="!showPassword" placeholder="请再次输入新密码" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password2'" @blur="focusField = ''" />
					</view>
				</view>

				<view class="submit" @click="submit">
					<text class="submit-text">重 置 密 码</text>
				</view>

				<view class="foot-link">
					想起密码了？<text class="link" @click="goBack">返回登录</text>
				</view>
			</view>

			<view class="foot-note">© 2026 LUMEN · 极光实验室</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				contact: '',
				code: '',
				password: '',
				password2: '',
				showPassword: false,
				focusField: '',
				counting: false,
				seconds: 60,
				timer: null
			}
		},
		onUnload() {
			if (this.timer) clearInterval(this.timer)
		},
		methods: {
			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			sendCode() {
				if (this.counting) return
				if (!this.contact) return this.toast('请输入绑定账号')
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发送')
				this.counting = true
				this.seconds = 60
				this.timer = setInterval(() => {
					this.seconds--
					if (this.seconds <= 0) {
						clearInterval(this.timer)
						this.counting = false
					}
				}, 1000)
			},

			submit() {
				if (!this.contact) return this.toast('请输入绑定账号')
				if (!this.code) return this.toast('请输入验证码')
				if (!this.password) return this.toast('请输入新密码')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				// TODO: 调用后端重置密码接口，成功后返回登录页
				this.toast('密码已重置')
			},

			goBack() {
				uni.navigateBack()
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
