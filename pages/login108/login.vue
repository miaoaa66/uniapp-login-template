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
				<view class="head-title">欢迎回来</view>
				<view class="head-sub">登录你的 LUMEN 账户，继续未完成的工作。</view>
			</view>

			<view class="card">
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
						<text>记住登录状态</text>
					</view>
					<text class="link" @click="goForget">忘记密码？</text>
				</view>

				<view class="submit" @click="submit">
					<text class="submit-text">登 录</text>
				</view>

				<view class="alt-rule">
					<view class="line"></view>
					<text class="txt">或使用以下方式继续</text>
					<view class="line"></view>
				</view>

				<view class="social-row">
					<view class="social" @click="altLogin('微信')"><text>微信</text></view>
					<view class="social" @click="altLogin('Apple')"><text>Apple</text></view>
					<view class="social" @click="altLogin('邮箱')"><text>邮箱</text></view>
				</view>

				<view class="foot-link">
					还没有账户？<text class="link" @click="goRegister">立即创建</text>
				</view>

				<view class="agreement">
					<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
					<text>我已阅读并同意<text class="link">《服务条款》</text>与<text class="link">《隐私政策》</text></text>
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
				if (!this.agreed) return this.toast('请先阅读并同意服务条款')
				if (!this.account) return this.toast('请输入账号')
				if (!this.password) return this.toast('请输入登录密码')
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			altLogin(type) {
				this.toast(type + '登录开发中')
			},

			goRegister() {
				uni.navigateTo({
					url: '/pages/login108/register'
				})
			},

			goForget() {
				uni.navigateTo({
					url: '/pages/login108/forget'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
