<template>
	<view class="gb-page">
		<view class="gb-nav">
			<view class="gb-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="gb-nav__title">找回密码</text>
			<view class="gb-nav__led">
				<view class="gb-led"></view>
			</view>
		</view>

		<view class="gb-body">
			<view class="gb-shell">
				<view class="gb-shell__brand">
					<text class="gb-shell__en">PIXEL BOY</text>
					<text class="gb-shell__sub">CONTINUE? 10 9 8 7</text>
				</view>

				<view class="gb-screen">
					<view class="gb-head">
						<view class="gb-head__logo">
							<view class="pxi pxi-key"></view>
						</view>
						<view class="gb-head__meta">
							<text class="gb-head__en">RESET SAVE</text>
							<text class="gb-head__cn">重置关卡钥匙</text>
						</view>
					</view>

					<text class="gb-title">找回密码</text>
					<text class="gb-lead">用手机短信验证码重设登录密码</text>

					<view class="gb-steps">
						<view class="gb-step" :class="{ on: step >= 1 }">
							<view class="gb-step__n">1</view>
							<text>验证手机</text>
						</view>
						<view class="gb-step__line" :class="{ on: step >= 2 }"></view>
						<view class="gb-step" :class="{ on: step >= 2 }">
							<view class="gb-step__n">2</view>
							<text>重置密码</text>
						</view>
					</view>

					<view class="gb-form">
						<view class="gb-field">
							<view class="gb-field__ico"><view class="pxi pxi-phone"></view></view>
							<input class="gb-field__in gb-field__in--en" v-model="phone" type="number" placeholder="+86"
								placeholder-style="color:#6f8a4a" maxlength="11" />
							<view class="gb-code" :class="{ off: smsLeft > 0 }" @click="sendCode">
								{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}
							</view>
						</view>

						<view class="gb-cells">
							<input v-for="(c, i) in cells" :key="i" class="gb-cell" :class="{ dim: !c, on: focusIndex === i }"
								v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
						</view>

						<view class="gb-field">
							<view class="gb-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="gb-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）"
								placeholder-style="color:#6f8a4a" maxlength="20" />
							<view class="gb-eye" @click="showPwd = !showPwd">
								<view class="pxi pxi-eye"></view>
							</view>
						</view>

						<view class="gb-field">
							<view class="gb-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="gb-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码"
								placeholder-style="color:#6f8a4a" maxlength="20" />
						</view>

						<view class="gb-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>START · 重 置 密 码</text>
					</view>

						<view class="gb-foot">
							<text>想起密码了？</text>
							<text class="gb-foot__link" @click="goLogin">继续游戏</text>
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
				step: 1,
				showPwd: false,
				phone: '',
				cells: ['', '', '', '', '', ''],
				focusIndex: 0,
				password: '',
				password2: '',
				smsLeft: 0,
				timer: null
			}
		},
		onLoad(options) {
			if (options && options.demo) {
				this.phone = '13800001234'
				this.cells = ['1', '9', '8', '9', '', '']
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

			onCell(i, e) {
				const v = (e.detail && e.detail.value) || ''
				if (v && i < 5) this.focusIndex = i + 1
			},

			sendCode() {
				if (this.smsLeft > 0) return
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
				this.smsLeft = 60
				this.focusIndex = 0
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
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
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
