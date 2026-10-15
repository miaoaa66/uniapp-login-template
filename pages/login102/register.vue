<template>
	<view class="ts-page">
		<view class="ts-nav">
			<view class="ts-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="ts-nav__title">注 册</text>
			<text class="ts-nav__slot">REC</text>
		</view>

		<view class="ts-body">
			<view class="ts-tape">
				<view class="ts-tape__win">
					<view class="ts-reel ts-reel--left"></view>
					<view class="ts-tape__label"><text class="ts-tape__t">MIXTAPE 1983 · SIDE A</text></view>
					<view class="ts-reel ts-reel--right"></view>
				</view>
				<view class="ts-tape__screw ts-tape__screw--lt"></view>
				<view class="ts-tape__screw ts-tape__screw--rt"></view>
				<view class="ts-tape__screw ts-tape__screw--lb"></view>
				<view class="ts-tape__screw ts-tape__screw--rb"></view>
			</view>
			<view class="ts-card">
				<view class="ts-brand">
					<view class="ts-brand__logo"><view class="pxi pxi-user"></view></view>
					<view class="ts-brand__meta">
						<text class="ts-brand__en">NEW TAPE</text>
						<text class="ts-brand__cn">录制新磁带</text>
					</view>
				</view>

				<text class="ts-title">注 册</text>
				<text class="ts-lead">录制一盒属于你的新磁带</text>

				<view class="ts-form">
					<view class="ts-field">
						<view class="ts-field__ico"><view class="pxi pxi-user"></view></view>
						<input class="ts-field__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#8b939c" maxlength="12" />
					</view>

					<view class="ts-field">
						<view class="ts-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="ts-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#8b939c" maxlength="40" />
					</view>

					<view class="ts-field">
						<view class="ts-field__ico"><view class="pxi pxi-shield"></view></view>
						<input class="ts-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#8b939c" maxlength="6" />
						<view class="ts-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="ts-field">
						<view class="ts-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="ts-field__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#8b939c" maxlength="20" />
						<view class="ts-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="ts-field">
						<view class="ts-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="ts-field__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#8b939c" maxlength="20" />
					</view>

					<view class="ts-power">
						<text class="ts-power__t">密码强度</text>
						<view class="ts-power__bars">
							<view class="ts-power__bar" :class="{ on: strength >= 1 }"></view>
							<view class="ts-power__bar" :class="{ on: strength >= 2 }"></view>
							<view class="ts-power__bar" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="ts-power__v">{{ strengthText }}</text>
					</view>

					<view class="ts-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>开 始 录 制</text>
					</view>

					<view class="ts-foot">
						<text>已有磁带？</text>
						<text class="ts-foot__link" @click="goLogin">返回登录</text>
					</view>

					<view class="ts-agree" @click="agreed = !agreed">
						<view class="ts-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="ts-link">《用户协议》</text>与<text class="ts-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="ts-wave">
				<view v-for="n in 24" :key="n" class="ts-wave__b" :class="'ts-wave__b--' + (n % 6)"></view>
			</view>
			<view class="ts-keys">
				<view class="ts-key">&lt;&lt;</view>
				<view class="ts-key ts-key--play">&gt;</view>
				<view class="ts-key">[]</view>
				<view class="ts-key">&gt;&gt;</view>
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
