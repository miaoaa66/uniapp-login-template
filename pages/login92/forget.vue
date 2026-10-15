<template>
	<view class="fc-page">
		<view class="fc-nav">
			<view class="fc-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="fc-nav__title">找回密码</text>
			<text class="fc-nav__slot">1983</text>
		</view>

		<view class="fc-body">
			<view class="fc-plate">
				<view class="fc-led"></view>
				<text class="fc-plate__t">CONTINUE? 10 9 8 7</text>
				<text class="fc-plate__tag">RETRY</text>
			</view>

			<view class="fc-card">
				<view class="fc-brand">
					<view class="fc-brand__logo">
						<view class="pxi pxi-key"></view>
					</view>
					<view class="fc-brand__meta">
						<text class="fc-brand__en">RESET KEY</text>
						<text class="fc-brand__cn">重置关卡钥匙</text>
					</view>
				</view>

				<text class="fc-title">找回密码</text>
				<text class="fc-lead">通过邮箱验证码重设你的登录密码</text>

				<!-- 步骤条 -->
				<view class="fc-steps">
					<view class="fc-step" :class="{ on: step >= 1 }">
						<view class="fc-step__n">1</view>
						<text>验证邮箱</text>
					</view>
					<view class="fc-step__line" :class="{ on: step >= 2 }"></view>
					<view class="fc-step" :class="{ on: step >= 2 }">
						<view class="fc-step__n">2</view>
						<text>重置密码</text>
					</view>
				</view>

				<view class="fc-form">
					<view class="fc-field">
						<view class="fc-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="fc-field__in" v-model="email" placeholder="注册时使用的邮箱"
							placeholder-style="color:#a8abb4" maxlength="40" />
						<view class="fc-code" :class="{ off: mailLeft > 0 }" @click="sendCode">
							{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}
						</view>
					</view>

					<view class="fc-cells">
						<input v-for="(c, i) in cells" :key="i" class="fc-cell" :class="{ dim: !c, on: focusIndex === i }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>

					<view class="fc-field">
						<view class="fc-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="fc-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）"
							placeholder-style="color:#a8abb4" maxlength="20" />
						<view class="fc-eye" @click="showPwd = !showPwd">
							<view class="pxi pxi-eye"></view>
						</view>
					</view>

					<view class="fc-field">
						<view class="fc-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="fc-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码"
							placeholder-style="color:#a8abb4" maxlength="20" />
					</view>

					<view class="fc-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>重 置 密 码</text>
					</view>

					<view class="fc-foot">
						<text>想起密码了？</text>
						<text class="fc-foot__link" @click="goLogin">返回登录</text>
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

			// 输入后自动跳下一格
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
