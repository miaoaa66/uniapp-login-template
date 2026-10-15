<template>
	<view class="cr-page">
		<!-- 终端标题条 -->
		<view class="cr-term">
			<view class="cr-term__dot cr-term__dot--1"></view>
			<view class="cr-term__dot cr-term__dot--2"></view>
			<view class="cr-term__dot cr-term__dot--3"></view>
			<text class="cr-term__t">PIXEL-ID://AUTH/LOGIN</text>
		</view>

		<!-- 导航栏 -->
		<view class="cr-nav">
			<view class="cr-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="cr-nav__title">登 录</text>
			<text class="cr-nav__slot">v2.0</text>
		</view>

		<view class="cr-body">
			<view class="cr-status">
				<view class="cr-status__led"></view>
				<text class="cr-status__t">SECURE CHANNEL ESTABLISHED</text>
				<text class="cr-status__v">256-BIT</text>
			</view>

			<view class="cr-panel">
				<view class="cr-brand">
					<view class="cr-brand__logo">
						<view class="pxi pxi-chip"></view>
					</view>
					<view class="cr-brand__meta">
						<text class="cr-brand__en">PIXEL ID</text>
						<text class="cr-brand__cn">像素通行证 · 认证节点</text>
					</view>
				</view>

				<text class="cr-title">登 录</text>
				<text class="cr-lead--en">&gt; AWAITING CREDENTIALS_</text>
				<text class="cr-lead">选择认证方式，接入你的像素身份</text>

				<view class="cr-tabs">
					<view v-for="(t, i) in tabs" :key="i" class="cr-tab" :class="{ on: tab === i }" @click="tab = i">
						{{ t }}
					</view>
				</view>

				<view class="cr-form">
					<!-- 密码登录 -->
					<block v-if="tab === 0">
						<view class="cr-field">
							<view class="cr-field__ico"><view class="pxi pxi-user"></view></view>
							<input class="cr-field__in" v-model="account" placeholder="手机号 / 邮箱"
								placeholder-style="color:#5a6090" maxlength="40" />
						</view>
						<view class="cr-field">
							<view class="cr-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="cr-field__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码"
								placeholder-style="color:#5a6090" maxlength="20" />
							<view class="cr-eye" @click="showPwd = !showPwd">
								<view class="pxi pxi-eye"></view>
							</view>
						</view>
						<view class="cr-row">
							<text class="cr-row__link" @click="goForget">忘记密码？</text>
						</view>
					</block>

					<!-- 手机验证码登录 -->
					<block v-else-if="tab === 1">
						<view class="cr-field">
							<view class="cr-field__ico"><view class="pxi pxi-phone"></view></view>
							<input class="cr-field__in cr-field__in--en" v-model="phone" type="number" placeholder="+86"
								placeholder-style="color:#5a6090" maxlength="11" />
						</view>
						<view class="cr-field">
							<view class="cr-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="cr-field__in" v-model="smsCode" type="number" placeholder="短信验证码"
								placeholder-style="color:#5a6090" maxlength="6" />
							<view class="cr-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">
								{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}
							</view>
						</view>
					</block>

					<!-- 邮箱验证码登录 -->
					<block v-else>
						<view class="cr-field">
							<view class="cr-field__ico"><view class="pxi pxi-mail"></view></view>
							<input class="cr-field__in" v-model="email" placeholder="player@pixel.id"
								placeholder-style="color:#5a6090" maxlength="40" />
						</view>
						<view class="cr-field">
							<view class="cr-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="cr-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码"
								placeholder-style="color:#5a6090" maxlength="6" />
							<view class="cr-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">
								{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}
							</view>
						</view>
					</block>

					<view class="cr-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>接 入 系 统</text>
					</view>

					<view class="cr-foot">
						<text>还没有身份节点？</text>
						<text class="cr-foot__link" @click="goRegister">立即注册</text>
					</view>

					<view class="cr-agree" @click="agreed = !agreed">
						<view class="cr-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="cr-link">《用户协议》</text>与<text class="cr-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>

			<view class="cr-hud">
				<text>© 2026 PIXEL-ID LAB</text>
				<text>NODE 0x1983</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabs: ['密码登录', '手机验证码', '邮箱验证码'],
				tab: 0,
				agreed: false,
				showPwd: false,
				account: '',
				password: '',
				phone: '',
				smsCode: '',
				email: '',
				mailCode: '',
				smsLeft: 0,
				mailLeft: 0,
				timer: null
			}
		},
		onLoad(options) {
			if (options && options.tab) this.tab = Number(options.tab) || 0
			if (options && options.demo) this.fillDemo()
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			fillDemo() {
				this.agreed = true
				this.account = 'player@pixel.id'
				this.password = 'n30n-1983'
				this.phone = '13800001234'
				this.email = 'player@pixel.id'
				this.smsCode = '1983'
				this.mailCode = '1983'
			},

			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			isPhone(v) {
				return /^1\d{10}$/.test(v)
			},

			isEmail(v) {
				return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v)
			},

			sendCode(type) {
				if (type === 'sms') {
					if (this.smsLeft > 0) return
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
					this.smsLeft = 60
				} else {
					if (this.mailLeft > 0) return
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					this.mailLeft = 60
				}
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发送')
				this.startTimer()
			},

			startTimer() {
				if (this.timer) return
				this.timer = setInterval(() => {
					if (this.smsLeft > 0) this.smsLeft--
					if (this.mailLeft > 0) this.mailLeft--
					if (this.smsLeft <= 0 && this.mailLeft <= 0) this.clearTimer()
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
				if (this.tab === 0) {
					if (!this.account) return this.toast('请输入账号')
					if (!this.password) return this.toast('请输入登录密码')
				} else if (this.tab === 1) {
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
					if (!this.smsCode) return this.toast('请输入短信验证码')
				} else {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (!this.mailCode) return this.toast('请输入邮箱验证码')
				}
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goBack() {
				uni.navigateBack()
			},

			goRegister() {
				uni.navigateTo({
					url: '/pages/login94/register'
				})
			},

			goForget() {
				uni.navigateTo({
					url: '/pages/login94/forget'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
