<template>
	<view class="sp-page">
		<view class="sp-nav">
			<view class="sp-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="sp-nav__title">注 册</text>
			<text class="sp-nav__slot">ENLIST</text>
		</view>

		<view class="sp-body">
			<view class="sp-radar">
				<view class="sp-radar__ring"></view>
				<view class="sp-radar__ring sp-radar__ring--inner"></view>
				<view class="sp-radar__cross"></view>
				<view class="sp-radar__sweep"></view>
				<view class="sp-radar__blip sp-radar__blip--a"></view>
				<view class="sp-radar__blip sp-radar__blip--b"></view>
			</view>
			<view class="sp-card">
				<view class="sp-brand">
					<view class="sp-brand__logo"><view class="pxi pxi-user"></view></view>
					<view class="sp-brand__meta">
						<text class="sp-brand__en">ENLIST</text>
						<text class="sp-brand__cn">登记船员</text>
					</view>
				</view>

				<text class="sp-title">注 册</text>
				<text class="sp-lead">登记身份，领取你的船员编号</text>

				<view class="sp-form">
					<view class="sp-field">
						<view class="sp-field__ico"><view class="pxi pxi-user"></view></view>
						<input class="sp-field__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#3f7a63" maxlength="12" />
					</view>

					<view class="sp-field">
						<view class="sp-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="sp-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#3f7a63" maxlength="40" />
					</view>

					<view class="sp-field">
						<view class="sp-field__ico"><view class="pxi pxi-shield"></view></view>
						<input class="sp-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#3f7a63" maxlength="6" />
						<view class="sp-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="sp-field">
						<view class="sp-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="sp-field__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#3f7a63" maxlength="20" />
						<view class="sp-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="sp-field">
						<view class="sp-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="sp-field__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#3f7a63" maxlength="20" />
					</view>

					<view class="sp-power">
						<text class="sp-power__t">密码强度</text>
						<view class="sp-power__bars">
							<view class="sp-power__bar" :class="{ on: strength >= 1 }"></view>
							<view class="sp-power__bar" :class="{ on: strength >= 2 }"></view>
							<view class="sp-power__bar" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="sp-power__v">{{ strengthText }}</text>
					</view>

					<view class="sp-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>提 交 登 记</text>
					</view>

					<view class="sp-foot">
						<text>已有编号？</text>
						<text class="sp-foot__link" @click="goLogin">返回登录</text>
					</view>

					<view class="sp-agree" @click="agreed = !agreed">
						<view class="sp-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="sp-link">《用户协议》</text>与<text class="sp-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="sp-hud">
				<view class="sp-hud__col">
					<text class="sp-hud__l">SECTOR 07</text>
					<text class="sp-hud__l">BEARING 214°</text>
				</view>
				<view class="sp-hud__col sp-hud__col--r">
					<text class="sp-hud__l">X 128.44</text>
					<text class="sp-hud__l">Y 902.10</text>
				</view>
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
				this.nickname = 'PLAYER-1'
				this.email = 'player@pixel.id'
				this.mailCode = '1983'
				this.password = '1up-1983'
				this.password2 = '1up-1983'
			}
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			toast(title) {
				uni.showToast({ title, icon: 'none' })
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
