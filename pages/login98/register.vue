<template>
	<view class="tw-page">
		<view class="tw-nav">
			<view class="tw-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="tw-nav__title">注 册</text>
			<text class="tw-nav__slot">NEW</text>
		</view>

		<view class="tw-body">
			<view class="tw-head">
				<text class="tw-head__no">FORM NO.1983</text>
				<text class="tw-head__t">PIXEL TYPEWRITER CO.</text>
			</view>
			<view class="tw-rule"></view>
			<view class="tw-card">
				<view class="tw-brand">
					<view class="tw-brand__logo"><view class="pxi pxi-user"></view></view>
					<view class="tw-brand__meta">
						<text class="tw-brand__en">NEW RECORD</text>
						<text class="tw-brand__cn">新建档案</text>
					</view>
				</view>

				<text class="tw-title">注 册</text>
				<text class="tw-lead">填写信息，建立你的第一条记录</text>

				<view class="tw-form">
					<view class="tw-field">
						<view class="tw-field__ico"><view class="pxi pxi-user"></view></view>
						<input class="tw-field__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#a49c8a" maxlength="12" />
					</view>

					<view class="tw-field">
						<view class="tw-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="tw-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#a49c8a" maxlength="40" />
					</view>

					<view class="tw-field">
						<view class="tw-field__ico"><view class="pxi pxi-shield"></view></view>
						<input class="tw-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#a49c8a" maxlength="6" />
						<view class="tw-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="tw-field">
						<view class="tw-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="tw-field__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#a49c8a" maxlength="20" />
						<view class="tw-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="tw-field">
						<view class="tw-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="tw-field__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#a49c8a" maxlength="20" />
					</view>

					<view class="tw-power">
						<text class="tw-power__t">密码强度</text>
						<view class="tw-power__bars">
							<view class="tw-power__bar" :class="{ on: strength >= 1 }"></view>
							<view class="tw-power__bar" :class="{ on: strength >= 2 }"></view>
							<view class="tw-power__bar" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="tw-power__v">{{ strengthText }}</text>
					</view>

					<view class="tw-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>存 档</text>
					</view>

					<view class="tw-foot">
						<text>已有档案？</text>
						<text class="tw-foot__link" @click="goLogin">返回登录</text>
					</view>

					<view class="tw-agree" @click="agreed = !agreed">
						<view class="tw-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="tw-link">《用户协议》</text>与<text class="tw-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="tw-keys">
				<view v-for="n in 3" :key="n" class="tw-keys__row">
					<view v-for="m in 8" :key="m" class="tw-key"></view>
				</view>
			</view>
			<view class="tw-platen"></view>
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
