<template>
	<view class="am-page">
		<view class="am-nav">
			<view class="am-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="am-nav__title">注 册</text>
			<text class="am-nav__slot">adduser</text>
		</view>

		<view class="am-body">
			<view class="am-term">
				<view class="am-term__bar">
					<view class="am-term__dot"></view>
					<view class="am-term__dot"></view>
					<view class="am-term__dot"></view>
					<text class="am-term__title">AMBER-OS v1.0 / tty0</text>
				</view>
			</view>
			<view class="am-card">
				<view class="am-brand">
					<view class="am-brand__logo"><view class="pxi pxi-user"></view></view>
					<view class="am-brand__meta">
						<text class="am-brand__en">ADDUSER</text>
						<text class="am-brand__cn">新建终端账户</text>
					</view>
				</view>

				<text class="am-title">注 册</text>
				<text class="am-lead">创建账户并写入 /etc/passwd</text>

				<view class="am-form">
					<view class="am-field">
						<view class="am-field__ico"><view class="pxi pxi-user"></view></view>
						<input class="am-field__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#82600e" maxlength="12" />
					</view>

					<view class="am-field">
						<view class="am-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="am-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#82600e" maxlength="40" />
					</view>

					<view class="am-field">
						<view class="am-field__ico"><view class="pxi pxi-shield"></view></view>
						<input class="am-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#82600e" maxlength="6" />
						<view class="am-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="am-field">
						<view class="am-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="am-field__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#82600e" maxlength="20" />
						<view class="am-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="am-field">
						<view class="am-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="am-field__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#82600e" maxlength="20" />
					</view>

					<view class="am-power">
						<text class="am-power__t">密码强度</text>
						<view class="am-power__bars">
							<view class="am-power__bar" :class="{ on: strength >= 1 }"></view>
							<view class="am-power__bar" :class="{ on: strength >= 2 }"></view>
							<view class="am-power__bar" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="am-power__v">{{ strengthText }}</text>
					</view>

					<view class="am-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>写 入</text>
					</view>

					<view class="am-foot">
						<text>已有账户？</text>
						<text class="am-foot__link" @click="goLogin">返回登录</text>
					</view>

					<view class="am-agree" @click="agreed = !agreed">
						<view class="am-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="am-link">《用户协议》</text>与<text class="am-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="am-log">
				<text class="am-log__l">&gt; LOADING MODULES ....... [OK]</text>
				<text class="am-log__l">&gt; MOUNT /dev/pixel ........ [OK]</text>
				<text class="am-log__l">&gt; READY<span class="am-caret">_</span></text>
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
