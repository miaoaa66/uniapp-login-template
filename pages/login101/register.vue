<template>
	<view class="nt-page">
		<view class="nt-nav">
			<view class="nt-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="nt-nav__title">注 册</text>
			<text class="nt-nav__slot">NEW</text>
		</view>

		<view class="nt-body">
			<view class="nt-pin"></view>
			<view class="nt-strip">
				<text class="nt-strip__t">TODAY · 登录 / 注册</text>
			</view>
			<view class="nt-card">
				<view class="nt-brand">
					<view class="nt-brand__logo"><view class="pxi pxi-user"></view></view>
					<view class="nt-brand__meta">
						<text class="nt-brand__en">NEW NOTE</text>
						<text class="nt-brand__cn">新建便签</text>
					</view>
				</view>

				<text class="nt-title">注 册</text>
				<text class="nt-lead">写一张新便签，钉在计划板上</text>

				<view class="nt-form">
					<view class="nt-field">
						<view class="nt-field__ico"><view class="pxi pxi-user"></view></view>
						<input class="nt-field__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#9b7a4d" maxlength="12" />
					</view>

					<view class="nt-field">
						<view class="nt-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="nt-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#9b7a4d" maxlength="40" />
					</view>

					<view class="nt-field">
						<view class="nt-field__ico"><view class="pxi pxi-shield"></view></view>
						<input class="nt-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#9b7a4d" maxlength="6" />
						<view class="nt-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="nt-field">
						<view class="nt-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="nt-field__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#9b7a4d" maxlength="20" />
						<view class="nt-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="nt-field">
						<view class="nt-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="nt-field__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#9b7a4d" maxlength="20" />
					</view>

					<view class="nt-power">
						<text class="nt-power__t">密码强度</text>
						<view class="nt-power__bars">
							<view class="nt-power__bar" :class="{ on: strength >= 1 }"></view>
							<view class="nt-power__bar" :class="{ on: strength >= 2 }"></view>
							<view class="nt-power__bar" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="nt-power__v">{{ strengthText }}</text>
					</view>

					<view class="nt-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>钉 上 去</text>
					</view>

					<view class="nt-foot">
						<text>已有便签？</text>
						<text class="nt-foot__link" @click="goLogin">返回登录</text>
					</view>

					<view class="nt-agree" @click="agreed = !agreed">
						<view class="nt-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="nt-link">《用户协议》</text>与<text class="nt-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="nt-notes">
				<view class="nt-note nt-note--green">
					<text class="nt-note__t">记得勾选协议</text>
				</view>
				<view class="nt-note nt-note--pink">
					<text class="nt-note__t">验证码 60 秒</text>
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
