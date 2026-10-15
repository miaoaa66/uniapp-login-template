<template>
	<view class="cr-page">
		<view class="cr-term">
			<view class="cr-term__dot cr-term__dot--1"></view>
			<view class="cr-term__dot cr-term__dot--2"></view>
			<view class="cr-term__dot cr-term__dot--3"></view>
			<text class="cr-term__t">PIXEL-ID://AUTH/RESET-KEY</text>
		</view>

		<view class="cr-nav">
			<view class="cr-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="cr-nav__title">找回密码</text>
			<text class="cr-nav__slot">v2.0</text>
		</view>

		<view class="cr-body">
			<view class="cr-status">
				<view class="cr-status__led"></view>
				<text class="cr-status__t">RECOVERY PROTOCOL ACTIVE</text>
				<text class="cr-status__v">STEP {{ step }}/2</text>
			</view>

			<view class="cr-panel">
				<view class="cr-brand">
					<view class="cr-brand__logo">
						<view class="pxi pxi-key"></view>
					</view>
					<view class="cr-brand__meta">
						<text class="cr-brand__en">RESET KEY</text>
						<text class="cr-brand__cn">重置访问密钥</text>
					</view>
				</view>

				<text class="cr-title">找回密码</text>
				<text class="cr-lead--en">&gt; REQUEST OTP TO PROCEED_</text>
				<text class="cr-lead">验证邮箱验证码后即可重设登录密码</text>

				<view class="cr-steps">
					<view class="cr-step" :class="{ on: step >= 1 }">
						<view class="cr-step__n">1</view>
						<text>验证邮箱</text>
					</view>
					<view class="cr-step__line" :class="{ on: step >= 2 }"></view>
					<view class="cr-step" :class="{ on: step >= 2 }">
						<view class="cr-step__n">2</view>
						<text>重置密钥</text>
					</view>
				</view>

				<view class="cr-form">
					<view class="cr-field">
						<view class="cr-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="cr-field__in" v-model="email" placeholder="注册时使用的邮箱"
							placeholder-style="color:#5a6090" maxlength="40" />
						<view class="cr-code" :class="{ off: mailLeft > 0 }" @click="sendCode">
							{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}
						</view>
					</view>

					<view class="cr-cells">
						<input v-for="(c, i) in cells" :key="i" class="cr-cell" :class="{ dim: !c, on: focusIndex === i }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>

					<view class="cr-field">
						<view class="cr-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="cr-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）"
							placeholder-style="color:#5a6090" maxlength="20" />
						<view class="cr-eye" @click="showPwd = !showPwd">
							<view class="pxi pxi-eye"></view>
						</view>
					</view>

					<view class="cr-field">
						<view class="cr-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="cr-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码"
							placeholder-style="color:#5a6090" maxlength="20" />
					</view>

					<view class="cr-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>重 置 密 钥</text>
					</view>

					<view class="cr-foot">
						<text>密钥已找回？</text>
						<text class="cr-foot__link" @click="goLogin">返回登录</text>
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
				step: 1,
				showPwd: false,
				email: '',
				cells: ['', '', '', '', '', ''],
				focusIndex: 0,
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null
			}
		},
		onLoad(options) {
			if (options && options.demo) {
				this.email = 'player@pixel.id'
				this.cells = ['1', '9', '8', '3', '', '']
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

			onCell(i, e) {
				const v = (e.detail && e.detail.value) || ''
				if (v && i < 5) this.focusIndex = i + 1
			},

			sendCode() {
				if (this.mailLeft > 0) return
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				this.mailLeft = 60
				this.focusIndex = 0
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
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
				if (this.password.length < 8) return this.toast('新密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				// TODO: 调用后端重置密码接口
				this.toast('密码已重置')
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
