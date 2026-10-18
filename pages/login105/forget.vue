<template>
	<view class="vd-page">
		<view class="vd-nav">
			<view class="vd-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="vd-nav__title">找回密码</text>
			<text class="vd-nav__slot">REFUND</text>
		</view>

		<view class="vd-body">
			<view class="vd-light">
				<text class="vd-light__t">DRINKS 24H</text>
				<view class="vd-light__bars">
					<view v-for="n in 4" :key="n" class="vd-light__bar" :class="'vd-light__bar--' + (n % 2)"></view>
				</view>
			</view>
			<view class="vd-price">
				<text class="vd-price__l">ITEM A1</text>
				<text class="vd-price__r">¥150</text>
			</view>
			<view class="vd-card">
				<view class="vd-brand">
					<view class="vd-brand__logo"><view class="pxi pxi-key"></view></view>
					<view class="vd-brand__meta">
						<text class="vd-brand__en">REFUND</text>
						<text class="vd-brand__cn">退币口</text>
					</view>
				</view>

				<text class="vd-title">找回密码</text>
				<text class="vd-lead">验证邮箱后，退回被吞掉的那枚硬币</text>

				<view class="vd-steps">
					<view class="vd-step" :class="{ on: step >= 1 }">
						<view class="vd-step__n">1</view>
						<text>验证邮箱</text>
					</view>
					<view class="vd-step__line" :class="{ on: step >= 2 }"></view>
					<view class="vd-step" :class="{ on: step >= 2 }">
						<view class="vd-step__n">2</view>
						<text>重置密码</text>
					</view>
				</view>

				<view class="vd-form">
					<view class="vd-field">
						<view class="vd-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="vd-field__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#8b97a8" maxlength="40" />
						<view class="vd-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="vd-cells">
						<input v-for="(c, i) in cells" :key="i" class="vd-cell" :class="{ dim: !c, on: focusIndex === i }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>

					<view class="vd-field">
						<view class="vd-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="vd-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#8b97a8" maxlength="20" />
						<view class="vd-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="vd-field">
						<view class="vd-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="vd-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#8b97a8" maxlength="20" />
					</view>

					<view class="vd-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>退 币</text>
					</view>

					<view class="vd-foot">
						<text>想起密码了？</text>
						<text class="vd-foot__link" @click="goLogin">返回登录</text>
					</view>
				</view>
			</view>
			<view class="vd-shelf">
				<view v-for="n in 6" :key="n" class="vd-slot">
					<view class="vd-can" :class="'vd-can--' + (n % 3)"></view>
					<text class="vd-slot__no">{{ 'A' + n }}</text>
				</view>
			</view>
			<view class="vd-tray">
				<view class="vd-tray__mouth"></view>
				<text class="vd-tray__t">PUSH</text>
				<view class="vd-tray__arrow"><view class="pxi pxi-arrow"></view></view>
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
				this.toast('硬币已退还，请查收')
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
