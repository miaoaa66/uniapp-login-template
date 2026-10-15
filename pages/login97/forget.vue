<template>
	<view class="am-page">
		<view class="am-nav">
			<view class="am-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="am-nav__title">找回密码</text>
			<text class="am-nav__slot">passwd</text>
		</view>

		<view class="am-body">
			<view class="am-term">
				<view class="am-term__bar">
					<view class="am-term__dot"></view>
					<view class="am-term__dot"></view>
					<view class="am-term__dot"></view>
					<text class="am-term__title">AMBER-OS v1.0 / tty0</text>
				</view>
			</view>
			<view class="am-card">
				<view class="am-brand">
					<view class="am-brand__logo"><view class="pxi pxi-key"></view></view>
					<view class="am-brand__meta">
						<text class="am-brand__en">PASSWD</text>
						<text class="am-brand__cn">重置终端口令</text>
					</view>
				</view>

				<text class="am-title">找回密码</text>
				<text class="am-lead">验证邮箱后执行 passwd 重置口令</text>

				<view class="am-steps">
					<view class="am-step" :class="{ on: step >= 1 }">
						<view class="am-step__n">1</view>
						<text>验证邮箱</text>
					</view>
					<view class="am-step__line" :class="{ on: step >= 2 }"></view>
					<view class="am-step" :class="{ on: step >= 2 }">
						<view class="am-step__n">2</view>
						<text>重置密码</text>
					</view>
				</view>

				<view class="am-form">
					<view class="am-field">
						<view class="am-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="am-field__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#82600e" maxlength="40" />
						<view class="am-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="am-cells">
						<input v-for="(c, i) in cells" :key="i" class="am-cell" :class="{ dim: !c, on: focusIndex === i }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>

					<view class="am-field">
						<view class="am-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="am-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#82600e" maxlength="20" />
						<view class="am-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="am-field">
						<view class="am-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="am-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#82600e" maxlength="20" />
					</view>

					<view class="am-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>执 行</text>
					</view>

					<view class="am-foot">
						<text>想起口令了？</text>
						<text class="am-foot__link" @click="goLogin">返回登录</text>
					</view>
				</view>
			</view>
			<view class="am-log">
				<text class="am-log__l">&gt; LOADING MODULES ....... [OK]</text>
				<text class="am-log__l">&gt; MOUNT /dev/pixel ........ [OK]</text>
				<text class="am-log__l">&gt; READY<span class="am-caret">_</span></text>
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
