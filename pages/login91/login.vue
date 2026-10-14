<template>
	<view class="page">
		<view class="brand">
			<view class="logo">
				<view class="logo-inner"></view>
			</view>
			<text class="brand-name">云汇通</text>
		</view>

		<view class="head">
			<view class="head-title">欢迎回来</view>
			<view class="head-sub">使用账号密码登录，继续你的工作</view>
		</view>

		<view class="card">
			<view class="field">
				<text class="field-label">账号</text>
				<view class="field-box">
					<input v-model="account" placeholder="手机号 / 邮箱" maxlength="40" />
				</view>
			</view>

			<view class="field">
				<text class="field-label">密码</text>
				<view class="field-box">
					<input v-model="password" :password="!showPassword" placeholder="请输入登录密码" maxlength="20" />
					<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐藏' : '显示' }}</text>
				</view>
			</view>

			<view class="row-between">
				<text class="link" @click="goForget">忘记密码？</text>
			</view>

			<view class="submit-btn" @click="submit">登 录</view>

			<view class="divider"><text>其他登录方式</text></view>

			<view class="other-row">
				<view class="submit-btn plain" @click="goPhone">手机号验证码登录</view>
				<view class="submit-btn plain" @click="goEmail">邮箱验证码登录</view>
			</view>

			<view class="foot-link">
				还没有账号？<text @click="goRegister">立即注册</text>
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
				account: '',
				password: '',
				showPassword: false,
				agreed: false
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
					url: '/pages/login91/register'
				})
			},

			goForget() {
				uni.navigateTo({
					url: '/pages/login91/forget'
				})
			},

			goPhone() {
				uni.navigateTo({
					url: '/pages/login91/phone-login'
				})
			},

			goEmail() {
				uni.navigateTo({
					url: '/pages/login91/email-login'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
