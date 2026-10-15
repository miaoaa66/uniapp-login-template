<template>
	<view class="fc-page">
		<view class="fc-nav">
			<view class="fc-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="fc-nav__title">注 册</text>
			<text class="fc-nav__slot">1983</text>
		</view>

		<view class="fc-body">
			<view class="fc-plate">
				<view class="fc-led"></view>
				<text class="fc-plate__t">INSERT CARTRIDGE</text>
				<text class="fc-plate__tag">PLAYER 1</text>
			</view>

			<view class="fc-card">
				<view class="fc-brand">
					<view class="fc-brand__logo">
						<view class="pxi pxi-invader"></view>
					</view>
					<view class="fc-brand__meta">
						<text class="fc-brand__en">PIXEL ID</text>
						<text class="fc-brand__cn">创建通行证</text>
					</view>
				</view>

				<text class="fc-title">注 册</text>
				<text class="fc-lead">选择你的角色名，开始第一关</text>

				<view class="fc-form">
					<view class="fc-field">
						<view class="fc-field__ico"><view class="pxi pxi-user"></view></view>
						<input class="fc-field__in" v-model="nickname" placeholder="昵称（2-12 字）"
							placeholder-style="color:#a8abb4" maxlength="12" />
					</view>

					<view class="fc-field">
						<view class="fc-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="fc-field__in" v-model="email" placeholder="player@pixel.id"
							placeholder-style="color:#a8abb4" maxlength="40" />
					</view>

					<view class="fc-field">
						<view class="fc-field__ico"><view class="pxi pxi-shield"></view></view>
						<input class="fc-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码"
							placeholder-style="color:#a8abb4" maxlength="6" />
						<view class="fc-code" :class="{ off: mailLeft > 0 }" @click="sendCode">
							{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}
						</view>
					</view>

					<view class="fc-field">
						<view class="fc-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="fc-field__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）"
							placeholder-style="color:#a8abb4" maxlength="20" />
						<view class="fc-eye" @click="showPwd = !showPwd">
							<view class="pxi pxi-eye"></view>
						</view>
					</view>

					<view class="fc-field">
						<view class="fc-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="fc-field__in" v-model="password2" :password="!showPwd" placeholder="确认密码"
							placeholder-style="color:#a8abb4" maxlength="20" />
					</view>

					<view class="fc-power">
						<text class="fc-power__t">密码强度</text>
						<view class="fc-power__bars">
							<view class="fc-power__bar" :class="{ on: strength >= 1 }"></view>
							<view class="fc-power__bar" :class="{ on: strength >= 2 }"></view>
							<view class="fc-power__bar" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="fc-power__v">{{ strengthText }}</text>
					</view>

					<view class="fc-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>创 建 通 行 证</text>
					</view>

					<view class="fc-foot">
						<text>已有通行证？</text>
						<text class="fc-foot__link" @click="goLogin">返回登录</text>
					</view>

					<view class="fc-agree" @click="agreed = !agreed">
						<view class="fc-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="fc-link">《用户协议》</text>与<text class="fc-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>

			<view class="fc-deco">
				<view class="fc-carts">
					<view v-for="n in 9" :key="n" class="fc-carts__i"></view>
				</view>
				<view class="fc-pad">
					<view class="fc-dpad"></view>
					<view class="fc-hint">
						<text class="fc-hint__t">START</text>
						<text class="fc-hint__t">SELECT</text>
					</view>
					<view class="fc-ab">
						<view class="fc-rbtn">A</view>
						<view class="fc-rbtn">B</view>
					</view>
				</view>
				<view class="fc-hud">
					<text>© 2026 PIXEL-ID LAB</text>
					<text>1UP 000000</text>
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
