<template>
	<view class="ac-page">
		<view class="ac-nav">
			<view class="ac-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="ac-nav__title">找回密码</text>
			<text class="ac-nav__slot">RETRY</text>
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
					<view class="ac-brand__logo"><view class="pxi pxi-key"></view></view>
					<view class="ac-brand__meta">
						<text class="ac-brand__en">CONTINUE</text>
						<text class="ac-brand__cn">重置街机密码</text>
					</view>
				</view>

				<text class="ac-title">找回密码</text>
				<text class="ac-lead">验证邮箱后重设你的街机密码</text>

				<view class="ac-steps">
					<view class="ac-step" :class="{ on: step >= 1 }">
						<view class="ac-step__n">1</view>
						<text>验证邮箱</text>
					</view>
					<view class="ac-step__line" :class="{ on: step >= 2 }"></view>
					<view class="ac-step" :class="{ on: step >= 2 }">
						<view class="ac-step__n">2</view>
						<text>重置密码</text>
					</view>
				</view>

				<view class="ac-form">
					<view class="ac-field">
						<view class="ac-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="ac-field__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#8f7cc4" maxlength="40" />
						<view class="ac-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="ac-cells">
						<input v-for="(c, i) in cells" :key="i" class="ac-cell" :class="{ dim: !c, on: focusIndex === i }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>

					<view class="ac-field">
						<view class="ac-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="ac-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#8f7cc4" maxlength="20" />
						<view class="ac-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="ac-field">
						<view class="ac-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="ac-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#8f7cc4" maxlength="20" />
					</view>

					<view class="ac-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>CONTINUE</text>
					</view>

					<view class="ac-foot">
						<text>想起密码了？</text>
						<text class="ac-foot__link" @click="goLogin">返回登录</text>
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
