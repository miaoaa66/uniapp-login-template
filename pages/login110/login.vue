<template>
	<view class="page">
		<view class="deco deco-half"></view>
		<view class="deco deco-ring"></view>
		<view class="deco deco-cross"></view>
		<view class="deco deco-tri"></view>
		<view class="deco deco-wave"></view>

		<view class="content">
			<view class="brand pop pop-1">
				<view class="logo">
					<view class="logo-core"></view>
				</view>
				<text class="brand-name">泡泡校园</text>
				<text class="brand-sub">MEMPHIS CLUB</text>
			</view>

			<view class="head pop pop-2">
				<view class="head-title">欢迎回来</view>
				<view class="head-sub">用账号密码登录，继续你的校园派对。</view>
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
					<text class="submit-text">登 录</text>
				</view>

				<view class="alt-rule">
					<view class="line"></view>
					<text class="txt">其他登录方式</text>
					<view class="line"></view>
				</view>

				<view class="social-row">
					<view class="social" @click="goPhone"><text>手机号登录</text></view>
					<view class="social" @click="goEmail"><text>邮箱登录</text></view>
				</view>

				<view class="foot-link">
					还没有账号？<text class="link" @click="goRegister">立即注册</text>
				</view>

				<view class="agreement">
					<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
					<text>我已阅读并同意<text class="link">《用户协议》</text>与<text class="link">《隐私政策》</text></text>
				</view>
			</view>

			<view class="foot-note pop pop-4">© 2026 MEMPHIS CLUB · 泡泡校园</view>
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
				remember: false,
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
					url: '/pages/login110/register'
				})
			},

			goForget() {
				uni.navigateTo({
					url: '/pages/login110/forget'
				})
			},

			goPhone() {
				uni.navigateTo({
					url: '/pages/login110/phone-login'
				})
			},

			goEmail() {
				uni.navigateTo({
					url: '/pages/login110/email-login'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
