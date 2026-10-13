<template>
	<view class="page">
		<view class="hero">
			<view class="brand">
				<view class="logo"></view>
				<text class="brand-name">云汇通</text>
			</view>
			<view class="hero-title">欢迎回来</view>
			<view class="hero-sub">登录后即可继续使用全部服务</view>
		</view>

		<view class="card">
			<!-- 登录方式切换 -->
			<view class="tabs">
				<view v-for="(tab, index) in tabs" :key="index" class="tab" :class="{ active: activeTab === index }"
					@click="activeTab = index">{{ tab }}</view>
			</view>

			<view class="form">
				<!-- 账号密码登录 -->
				<block v-if="activeTab === 0">
					<view class="field">
						<text class="field-label">账号</text>
						<input v-model="account" placeholder="手机号 / 邮箱" maxlength="40" />
					</view>
					<view class="field">
						<text class="field-label">密码</text>
						<input v-model="password" :password="!showPassword" placeholder="请输入登录密码" maxlength="20" />
						<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐藏' : '显示' }}</text>
					</view>
					<view class="row-between">
						<text class="link" @click="goForget">忘记密码？</text>
					</view>
				</block>

				<!-- 手机号验证码登录 -->
				<block v-else-if="activeTab === 1">
					<view class="field">
						<text class="field-label">+86</text>
						<input v-model="phone" type="number" placeholder="请输入手机号" maxlength="11" />
					</view>
					<view class="field">
						<text class="field-label">验证码</text>
						<input v-model="smsCode" type="number" placeholder="请输入短信验证码" maxlength="6" />
						<text class="code-btn" :class="{ gray: phoneCountdown > 0 }" @click="sendCode('phone')">
							{{ codeText(phoneCountdown) }}
						</text>
					</view>
					<view class="row-between">
						<text class="link" @click="goForget">忘记密码？</text>
					</view>
				</block>

				<!-- 邮箱验证码登录 -->
				<block v-else>
					<view class="field">
						<text class="field-label">邮箱</text>
						<input v-model="email" placeholder="请输入邮箱地址" maxlength="40" />
					</view>
					<view class="field">
						<text class="field-label">验证码</text>
						<input v-model="emailCode" type="number" placeholder="请输入邮箱验证码" maxlength="6" />
						<text class="code-btn" :class="{ gray: emailCountdown > 0 }" @click="sendCode('email')">
							{{ codeText(emailCountdown) }}
						</text>
					</view>
					<view class="row-between">
						<text class="link" @click="goForget">忘记密码？</text>
					</view>
				</block>

				<view class="submit-btn" @click="submit">登 录</view>

				<view class="foot-link">
					还没有账号？<text @click="goRegister">立即注册</text>
				</view>

				<view class="agreement">
					<view class="checkbox" :class="{ checked: agreed }" @click="agreed = !agreed"></view>
					<text>我已阅读并同意<text class="link">《用户协议》</text>和<text class="link">《隐私政策》</text></text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabs: ['密码登录', '手机号登录', '邮箱登录'],
				activeTab: 0,
				showPassword: false,
				agreed: false,
				account: '',
				password: '',
				phone: '',
				smsCode: '',
				email: '',
				emailCode: '',
				phoneCountdown: 0,
				emailCountdown: 0,
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

			isEmail(value) {
				return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(value)
			},

			codeText(countdown) {
				return countdown > 0 ? countdown + 's' : '获取验证码'
			},

			// 发送验证码
			sendCode(type) {
				if (type === 'phone') {
					if (this.phoneCountdown > 0) return
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
					this.phoneCountdown = 60
				} else {
					if (this.emailCountdown > 0) return
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					this.emailCountdown = 60
				}
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发送')
				this.startTimer()
			},

			startTimer() {
				if (this.timer) return
				this.timer = setInterval(() => {
					if (this.phoneCountdown > 0) this.phoneCountdown--
					if (this.emailCountdown > 0) this.emailCountdown--
					if (this.phoneCountdown <= 0 && this.emailCountdown <= 0) this.clearTimer()
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
				if (this.activeTab === 0) {
					if (!this.account) return this.toast('请输入账号')
					if (!this.password) return this.toast('请输入登录密码')
				} else if (this.activeTab === 1) {
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
					if (!this.smsCode) return this.toast('请输入短信验证码')
				} else {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (!this.emailCode) return this.toast('请输入邮箱验证码')
				}
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goRegister() {
				uni.navigateTo({
					url: '/pages/login90/register'
				})
			},

			goForget() {
				uni.navigateTo({
					url: '/pages/login90/forget'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>