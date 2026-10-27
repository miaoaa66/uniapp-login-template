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
				<view class="head-title">欢迎回来</view>
				<view class="head-sub">捏一捏你的账号，继续今天的创作吧。</view>
			</view>

			<view class="card pop pop-3">
				<view class="field">
					<text class="field-label">账号</text>
					<view class="field-box" :class="{ active: focusField === 'account' }">
						<input class="input" v-model="account" placeholder="手机号 / 邮箱" placeholder-class="ph" maxlength="40"
							@focus="focusField = 'account'" @blur="focusField = ''" />
					</view>
				</view>

				<view class="field">
					<text class="field-label">密码</text>
					<view class="field-box" :class="{ active: focusField === 'password' }">
						<input class="input" v-model="password" :password="!showPassword" placeholder="请输入登录密码" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password'" @blur="focusField = ''" />
						<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐藏' : '显示' }}</text>
					</view>
				</view>

				<view class="row-between">
					<view class="remember" @click="remember = !remember">
						<view class="checkbox" :class="{ on: remember }"></view>
						<text>记住我</text>
					</view>
					<text class="link" @click="goForget">忘记密码？</text>
				</view>

				<view class="submit" @click="submit">
					<text class="submit-text">捏一捏登录</text>
				</view>

				<view class="foot-link">
					还没有账号？<text class="link" @click="goRegister">去捏一个</text>
				</view>

				<view class="agreement">
					<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
					<text>我已阅读并同意<text class="link">《用户协议》</text>与<text class="link">《隐私政策》</text></text>
				</view>
			</view>

			<view class="submit plain pop pop-4" @click="goPhoneLogin">
				<text class="submit-text">手机验证码登录</text>
			</view>
			<view class="submit plain" @click="goEmailLogin">
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
				account: '',
				password: '',
				showPassword: false,
				remember: true,
				agreed: false,
				focusField: ''
			}
		},
		methods: {
			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			submit() {
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				if (!this.account) return this.toast('请输入账号')
				if (!this.password) return this.toast('请输入登录密码')
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goRegister() {
				uni.navigateTo({
					url: '/pages/login128/register'
				})
			},

			goForget() {
				uni.navigateTo({
					url: '/pages/login128/forget'
				})
			},

			goPhoneLogin() {
				uni.navigateTo({
					url: '/pages/login128/phone-login'
				})
			},

			goEmailLogin() {
				uni.navigateTo({
					url: '/pages/login128/email-login'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
