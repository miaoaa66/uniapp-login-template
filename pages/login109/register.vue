<template>
	<view class="page">
		<view class="blob blob-1"></view>
		<view class="blob blob-2"></view>
		<view class="blob blob-3"></view>

		<view class="content">
			<view class="brand pop pop-1">
				<view class="logo">
					<view class="logo-core"></view>
				</view>
				<text class="brand-name">黏土星球</text>
				<text class="brand-sub">CLAY PLANET</text>
			</view>

			<view class="head pop pop-2">
				<view class="head-title">捏个新账号</view>
				<view class="head-sub">只需几步，就能拥有属于你的星球。</view>
			</view>

			<view class="card pop pop-3">
				<view class="field">
					<text class="field-label">昵称</text>
					<view class="field-box" :class="{ active: focusField === 'nickname' }">
						<input class="input" v-model="nickname" placeholder="给自己起个好听的名字" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'nickname'" @blur="focusField = ''" />
					</view>
				</view>

				<view class="field">
					<text class="field-label">手机号 / 邮箱</text>
					<view class="field-box" :class="{ active: focusField === 'account' }">
						<input class="input" v-model="account" placeholder="用于登录与找回密码" placeholder-class="ph" maxlength="40"
							@focus="focusField = 'account'" @blur="focusField = ''" />
					</view>
				</view>

				<view class="field">
					<text class="field-label">设置密码</text>
					<view class="field-box" :class="{ active: focusField === 'password' }">
						<input class="input" v-model="password" :password="!showPassword" placeholder="8-20 位，建议字母加数字" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password'" @blur="focusField = ''" />
						<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐藏' : '显示' }}</text>
					</view>
				</view>

				<view class="field">
					<text class="field-label">确认密码</text>
					<view class="field-box" :class="{ active: focusField === 'password2' }">
						<input class="input" v-model="password2" :password="!showPassword" placeholder="请再次输入密码" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password2'" @blur="focusField = ''" />
					</view>
				</view>

				<view class="submit" @click="submit">
					<text class="submit-text">完成注册</text>
				</view>

				<view class="foot-link">
					已有账号？<text class="link" @click="goLogin">返回登录</text>
				</view>

				<view class="agreement">
					<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
					<text>我已阅读并同意<text class="link">《用户协议》</text>与<text class="link">《隐私政策》</text></text>
				</view>
			</view>

			<view class="foot-note pop pop-4">© 2026 CLAY PLANET · 黏土星球实验室</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				nickname: '',
				account: '',
				password: '',
				password2: '',
				showPassword: false,
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
				if (!this.nickname) return this.toast('请输入昵称')
				if (!this.account) return this.toast('请输入手机号或邮箱')
				if (!this.password) return this.toast('请设置密码')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
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
