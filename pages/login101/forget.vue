<template>
	<view class="nt-page">
		<view class="nt-nav">
			<view class="nt-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="nt-nav__title">找回密码</text>
			<text class="nt-nav__slot">REDO</text>
		</view>

		<view class="nt-body">
			<view class="nt-pin"></view>
			<view class="nt-strip">
				<text class="nt-strip__t">TODAY · 登录 / 注册</text>
			</view>
			<view class="nt-card">
				<view class="nt-brand">
					<view class="nt-brand__logo"><view class="pxi pxi-key"></view></view>
					<view class="nt-brand__meta">
						<text class="nt-brand__en">REWRITE</text>
						<text class="nt-brand__cn">重写便签</text>
					</view>
				</view>

				<text class="nt-title">找回密码</text>
				<text class="nt-lead">验证邮箱后重写你的登录便签</text>

				<view class="nt-steps">
					<view class="nt-step" :class="{ on: step >= 1 }">
						<view class="nt-step__n">1</view>
						<text>验证邮箱</text>
					</view>
					<view class="nt-step__line" :class="{ on: step >= 2 }"></view>
					<view class="nt-step" :class="{ on: step >= 2 }">
						<view class="nt-step__n">2</view>
						<text>重置密码</text>
					</view>
				</view>

				<view class="nt-form">
					<view class="nt-field">
						<view class="nt-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="nt-field__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#9b7a4d" maxlength="40" />
						<view class="nt-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="nt-cells">
						<input v-for="(c, i) in cells" :key="i" class="nt-cell" :class="{ dim: !c, on: focusIndex === i }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>

					<view class="nt-field">
						<view class="nt-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="nt-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#9b7a4d" maxlength="20" />
						<view class="nt-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="nt-field">
						<view class="nt-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="nt-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#9b7a4d" maxlength="20" />
					</view>

					<view class="nt-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>重 写 便 签</text>
					</view>

					<view class="nt-foot">
						<text>想起密码了？</text>
						<text class="nt-foot__link" @click="goLogin">返回登录</text>
					</view>
				</view>
			</view>
			<view class="nt-notes">
				<view class="nt-note nt-note--green">
					<text class="nt-note__t">记得勾选协议</text>
				</view>
				<view class="nt-note nt-note--pink">
					<text class="nt-note__t">验证码 60 秒</text>
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
