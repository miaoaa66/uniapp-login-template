<template>
	<view class="ac-page">
		<view class="ac-nav">
			<view class="ac-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="ac-nav__title">注 册</text>
			<text class="ac-nav__slot">NEW</text>
		</view>

		<view class="ac-body">
			<view class="ac-marquee">
				<text class="ac-marquee__t">A R C A D E</text>
			</view>
			<view class="ac-coin">
				<text class="ac-coin__t">INSERT COIN</text>
				<view class="ac-coin__slot"></view>
				<view class="ac-coin__led"></view>
			</view>
			<view class="ac-card">
				<view class="ac-brand">
					<view class="ac-brand__logo"><view class="pxi pxi-user"></view></view>
					<view class="ac-brand__meta">
						<text class="ac-brand__en">NEW PLAYER</text>
						<text class="ac-brand__cn">建立街机档案</text>
					</view>
				</view>

				<text class="ac-title">注 册</text>
				<text class="ac-lead">登记你的名字，成为 1 号玩家</text>

				<view class="ac-form">
					<view class="ac-field">
						<view class="ac-field__ico"><view class="pxi pxi-user"></view></view>
						<input class="ac-field__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#8f7cc4" maxlength="12" />
					</view>

					<view class="ac-field">
						<view class="ac-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="ac-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#8f7cc4" maxlength="40" />
					</view>

					<view class="ac-field">
						<view class="ac-field__ico"><view class="pxi pxi-shield"></view></view>
						<input class="ac-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#8f7cc4" maxlength="6" />
						<view class="ac-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="ac-field">
						<view class="ac-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="ac-field__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#8f7cc4" maxlength="20" />
						<view class="ac-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="ac-field">
						<view class="ac-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="ac-field__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#8f7cc4" maxlength="20" />
					</view>

					<view class="ac-power">
						<text class="ac-power__t">密码强度</text>
						<view class="ac-power__bars">
							<view class="ac-power__bar" :class="{ on: strength >= 1 }"></view>
							<view class="ac-power__bar" :class="{ on: strength >= 2 }"></view>
							<view class="ac-power__bar" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="ac-power__v">{{ strengthText }}</text>
					</view>

					<view class="ac-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>PRESS START</text>
					</view>

					<view class="ac-foot">
						<text>已经有档案了？</text>
						<text class="ac-foot__link" @click="goLogin">返回登录</text>
					</view>

					<view class="ac-agree" @click="agreed = !agreed">
						<view class="ac-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="ac-link">《用户协议》</text>与<text class="ac-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="ac-panel">
				<view class="ac-stick"><view class="ac-stick__ball"></view></view>
				<view class="ac-btns">
					<view v-for="n in 6" :key="n" class="ac-key" :class="'ac-key--' + n"></view>
				</view>
			</view>
			<view class="ac-hud">
				<text class="ac-hud__l">1UP 004500</text>
				<text class="ac-hud__l">HIGH 012800</text>
				<text class="ac-hud__l">CREDIT 1</text>
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
