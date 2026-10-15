<template>
	<view class="gb-page">
		<view class="gb-nav">
			<view class="gb-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="gb-nav__title">新建存档</text>
			<view class="gb-nav__led">
				<view class="gb-led"></view>
			</view>
		</view>

		<view class="gb-body">
			<view class="gb-shell">
				<view class="gb-shell__brand">
					<text class="gb-shell__en">PIXEL BOY</text>
					<text class="gb-shell__sub">NEW GAME PLAYER 1</text>
				</view>

				<view class="gb-screen">
					<view class="gb-head">
						<view class="gb-head__logo">
							<view class="pxi pxi-invader"></view>
						</view>
						<view class="gb-head__meta">
							<text class="gb-head__en">PIXEL ID</text>
							<text class="gb-head__cn">创建通行证</text>
						</view>
					</view>

					<text class="gb-title">注 册</text>
					<text class="gb-lead">输入手机号，短信验证码开场</text>

					<view class="gb-form">
						<view class="gb-field">
							<view class="gb-field__ico"><view class="pxi pxi-user"></view></view>
							<input class="gb-field__in" v-model="nickname" placeholder="昵称（2-12 字）"
								placeholder-style="color:#6f8a4a" maxlength="12" />
						</view>

						<view class="gb-field">
							<view class="gb-field__ico"><view class="pxi pxi-phone"></view></view>
							<input class="gb-field__in gb-field__in--en" v-model="phone" type="number" placeholder="+86"
								placeholder-style="color:#6f8a4a" maxlength="11" />
						</view>

						<view class="gb-field">
							<view class="gb-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="gb-field__in" v-model="smsCode" type="number" placeholder="短信验证码"
								placeholder-style="color:#6f8a4a" maxlength="6" />
							<view class="gb-code" :class="{ off: smsLeft > 0 }" @click="sendCode">
								{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}
							</view>
						</view>

						<view class="gb-field">
							<view class="gb-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="gb-field__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）"
								placeholder-style="color:#6f8a4a" maxlength="20" />
							<view class="gb-eye" @click="showPwd = !showPwd">
								<view class="pxi pxi-eye"></view>
							</view>
						</view>

						<view class="gb-field">
							<view class="gb-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="gb-field__in" v-model="password2" :password="!showPwd" placeholder="确认密码"
								placeholder-style="color:#6f8a4a" maxlength="20" />
						</view>

						<view class="gb-power">
							<text class="gb-power__t">强度</text>
							<view class="gb-power__bars">
								<view class="gb-power__bar" :class="{ on: strength >= 1 }"></view>
								<view class="gb-power__bar" :class="{ on: strength >= 2 }"></view>
								<view class="gb-power__bar" :class="{ on: strength >= 3 }"></view>
							</view>
							<text class="gb-power__v">{{ strengthText }}</text>
						</view>

						<view class="gb-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>START · 开 始 游 戏</text>
					</view>

						<view class="gb-foot">
							<text>已有存档？</text>
							<text class="gb-foot__link" @click="goLogin">继续游戏</text>
						</view>

						<view class="gb-agree" @click="agreed = !agreed">
							<view class="gb-check" :class="{ on: agreed }"></view>
							<text>我已阅读并同意<text class="gb-link">《用户协议》</text>与<text class="gb-link">《隐私政策》</text></text>
						</view>
					</view>
				</view>

				<view class="gb-ctrl">
					<view class="gb-dpad">
						<view class="gb-dpad__h"></view>
						<view class="gb-dpad__v"></view>
						<view class="gb-dpad__c"></view>
					</view>
					<view class="gb-btns">
						<view class="gb-abtn">B</view>
						<view class="gb-abtn">A</view>
					</view>
				</view>

				<view class="gb-speaker">
					<view v-for="n in 5" :key="n" class="gb-speaker__i"></view>
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
				phone: '',
				smsCode: '',
				password: '',
				password2: '',
				smsLeft: 0,
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
				this.phone = '13800001234'
				this.smsCode = '1989'
				this.password = 'dmG-1989'
				this.password2 = 'dmG-1989'
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

			isPhone(v) {
				return /^1\d{10}$/.test(v)
			},

			sendCode() {
				if (this.smsLeft > 0) return
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
				this.smsLeft = 60
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发送')
				if (this.timer) return
				this.timer = setInterval(() => {
					if (this.smsLeft > 0) this.smsLeft--
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
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
				if (!this.smsCode) return this.toast('请输入短信验证码')
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
