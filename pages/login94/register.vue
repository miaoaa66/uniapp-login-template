<template>
	<view class="cr-page">
		<view class="cr-term">
			<view class="cr-term__dot cr-term__dot--1"></view>
			<view class="cr-term__dot cr-term__dot--2"></view>
			<view class="cr-term__dot cr-term__dot--3"></view>
			<text class="cr-term__t">PIXEL-ID://AUTH/REGISTER</text>
		</view>

		<view class="cr-nav">
			<view class="cr-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="cr-nav__title">注 册</text>
			<text class="cr-nav__slot">v2.0</text>
		</view>

		<view class="cr-body">
			<view class="cr-status">
				<view class="cr-status__led"></view>
				<text class="cr-status__t">NEW NODE PROVISIONING</text>
				<text class="cr-status__v">IDLE</text>
			</view>

			<view class="cr-panel">
				<view class="cr-brand">
					<view class="cr-brand__logo">
						<view class="pxi pxi-chip"></view>
					</view>
					<view class="cr-brand__meta">
						<text class="cr-brand__en">PIXEL ID</text>
						<text class="cr-brand__cn">创建认证节点</text>
					</view>
				</view>

				<text class="cr-title">注 册</text>
				<text class="cr-lead--en">&gt; CREATE NEW IDENTITY_</text>
				<text class="cr-lead">填写邮箱并完成验证，生成你的像素身份</text>

				<view class="cr-form">
					<view class="cr-field">
						<view class="cr-field__ico"><view class="pxi pxi-user"></view></view>
						<input class="cr-field__in" v-model="nickname" placeholder="昵称（2-12 字）"
							placeholder-style="color:#5a6090" maxlength="12" />
					</view>

					<view class="cr-field">
						<view class="cr-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="cr-field__in" v-model="email" placeholder="player@pixel.id"
							placeholder-style="color:#5a6090" maxlength="40" />
					</view>

					<view class="cr-field">
						<view class="cr-field__ico"><view class="pxi pxi-shield"></view></view>
						<input class="cr-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码"
							placeholder-style="color:#5a6090" maxlength="6" />
						<view class="cr-code" :class="{ off: mailLeft > 0 }" @click="sendCode">
							{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}
						</view>
					</view>

					<view class="cr-field">
						<view class="cr-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="cr-field__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）"
							placeholder-style="color:#5a6090" maxlength="20" />
						<view class="cr-eye" @click="showPwd = !showPwd">
							<view class="pxi pxi-eye"></view>
						</view>
					</view>

					<view class="cr-field">
						<view class="cr-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="cr-field__in" v-model="password2" :password="!showPwd" placeholder="确认密码"
							placeholder-style="color:#5a6090" maxlength="20" />
					</view>

					<view class="cr-power">
						<text class="cr-power__t">强度</text>
						<view class="cr-power__bars">
							<view class="cr-power__bar" :class="{ on: strength >= 1 }"></view>
							<view class="cr-power__bar" :class="{ on: strength >= 2 }"></view>
							<view class="cr-power__bar" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="cr-power__v">{{ strengthText }}</text>
					</view>

					<view class="cr-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>生 成 身 份</text>
					</view>

					<view class="cr-foot">
						<text>已有认证节点？</text>
						<text class="cr-foot__link" @click="goLogin">返回登录</text>
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
				agreed: false,
				showPwd: false,
				nickname: '',
				email: '',
				mailCode: '',
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null
			}
		},
		computed: {
			strength() {
				const v = this.password
				if (!v) return 0
				let s = 0
				if (v.length >= 8) s++
				if (/[a-zA-Z]/.test(v) && /\d/.test(v)) s++
				if (/[^a-zA-Z0-9]/.test(v)) s++
				return s
			},
			strengthText() {
				return ['──', '弱', '中', '强'][this.strength]
			}
		},
		onLoad(options) {
			if (options && options.demo) {
				this.agreed = true
				this.nickname = 'NODE-01'
				this.email = 'player@pixel.id'
				this.mailCode = '1983'
				this.password = 'n30n-1983'
				this.password2 = 'n30n-1983'
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

			sendCode() {
				if (this.mailLeft > 0) return
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				this.mailLeft = 60
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发送')
				if (this.timer) return
				this.timer = setInterval(() => {
					if (this.mailLeft > 0) this.mailLeft--
					else this.clearTimer()
				}, 1000)
			},

			clearTimer() {
				if (this.timer) {
					clearInterval(this.timer)
					this.timer = null
				}
			},

			submit() {
				if (!this.nickname) return this.toast('请输入昵称')
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (!this.mailCode) return this.toast('请输入邮箱验证码')
				if (this.password.length < 8) return this.toast('密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端注册接口
				this.toast('注册成功')
			},

			goBack() {
				uni.navigateBack()
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
