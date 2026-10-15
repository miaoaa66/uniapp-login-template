<template>
	<view class="sp-page">
		<view class="sp-nav">
			<view class="sp-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="sp-nav__title">找回密码</text>
			<text class="sp-nav__slot">RESET</text>
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
					<view class="sp-brand__logo"><view class="pxi pxi-key"></view></view>
					<view class="sp-brand__meta">
						<text class="sp-brand__en">RECOVER</text>
						<text class="sp-brand__cn">恢复访问</text>
					</view>
				</view>

				<text class="sp-title">找回密码</text>
				<text class="sp-lead">核验邮箱后恢复你的访问密钥</text>

				<view class="sp-steps">
					<view class="sp-step" :class="{ on: step >= 1 }">
						<view class="sp-step__n">1</view>
						<text>验证邮箱</text>
					</view>
					<view class="sp-step__line" :class="{ on: step >= 2 }"></view>
					<view class="sp-step" :class="{ on: step >= 2 }">
						<view class="sp-step__n">2</view>
						<text>重置密码</text>
					</view>
				</view>

				<view class="sp-form">
					<view class="sp-field">
						<view class="sp-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="sp-field__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#3f7a63" maxlength="40" />
						<view class="sp-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="sp-cells">
						<input v-for="(c, i) in cells" :key="i" class="sp-cell" :class="{ dim: !c, on: focusIndex === i }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>

					<view class="sp-field">
						<view class="sp-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="sp-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#3f7a63" maxlength="20" />
						<view class="sp-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="sp-field">
						<view class="sp-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="sp-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#3f7a63" maxlength="20" />
					</view>

					<view class="sp-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>恢 复 密 钥</text>
					</view>

					<view class="sp-foot">
						<text>想起密码了？</text>
						<text class="sp-foot__link" @click="goLogin">返回登录</text>
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
				uni.showToast({ title, icon: 'none' })
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
