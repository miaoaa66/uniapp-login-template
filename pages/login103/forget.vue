<template>
	<view class="ca-page">
		<view class="ca-nav">
			<view class="ca-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="ca-nav__title">找回密码</text>
			<text class="ca-nav__slot">RESET</text>
		</view>

		<view class="ca-body">
			<view class="ca-month">
				<text class="ca-month__m">OCT</text>
				<text class="ca-month__y">2026</text>
			</view>
			<view class="ca-card">
				<view class="ca-brand">
					<view class="ca-brand__logo"><view class="pxi pxi-key"></view></view>
					<view class="ca-brand__meta">
						<text class="ca-brand__en">RESET</text>
						<text class="ca-brand__cn">重置打卡密码</text>
					</view>
				</view>

				<text class="ca-title">找回密码</text>
				<text class="ca-lead">验证邮箱后重设你的打卡密码</text>

				<view class="ca-steps">
					<view class="ca-step" :class="{ on: step >= 1 }">
						<view class="ca-step__n">1</view>
						<text>验证邮箱</text>
					</view>
					<view class="ca-step__line" :class="{ on: step >= 2 }"></view>
					<view class="ca-step" :class="{ on: step >= 2 }">
						<view class="ca-step__n">2</view>
						<text>重置密码</text>
					</view>
				</view>

				<view class="ca-form">
					<view class="ca-field">
						<view class="ca-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="ca-field__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#9aa3af" maxlength="40" />
						<view class="ca-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="ca-cells">
						<input v-for="(c, i) in cells" :key="i" class="ca-cell" :class="{ dim: !c, on: focusIndex === i }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>

					<view class="ca-field">
						<view class="ca-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="ca-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#9aa3af" maxlength="20" />
						<view class="ca-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="ca-field">
						<view class="ca-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="ca-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#9aa3af" maxlength="20" />
					</view>

					<view class="ca-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>重 置 密 码</text>
					</view>

					<view class="ca-foot">
						<text>想起密码了？</text>
						<text class="ca-foot__link" @click="goLogin">返回登录</text>
					</view>
				</view>
			</view>
			<view class="ca-week">
				<text v-for="(w, i) in week" :key="i" class="ca-week__d">{{ w }}</text>
			</view>
			<view class="ca-grid">
				<view v-for="n in 28" :key="n" class="ca-day"
					:class="{ 'ca-day--today': n === 14, 'ca-day--past': n < 14, 'ca-day--off': n > 21 }">{{ n }}</view>
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
				timer: null,
				week: ["日","一","二","三","四","五","六"]
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
