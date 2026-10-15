<template>
	<view class="ts-page">
		<view class="ts-nav">
			<view class="ts-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="ts-nav__title">找回密码</text>
			<text class="ts-nav__slot">ERASE</text>
		</view>

		<view class="ts-body">
			<view class="ts-tape">
				<view class="ts-tape__win">
					<view class="ts-reel ts-reel--left"></view>
					<view class="ts-tape__label"><text class="ts-tape__t">MIXTAPE 1983 · SIDE A</text></view>
					<view class="ts-reel ts-reel--right"></view>
				</view>
				<view class="ts-tape__screw ts-tape__screw--lt"></view>
				<view class="ts-tape__screw ts-tape__screw--rt"></view>
				<view class="ts-tape__screw ts-tape__screw--lb"></view>
				<view class="ts-tape__screw ts-tape__screw--rb"></view>
			</view>
			<view class="ts-card">
				<view class="ts-brand">
					<view class="ts-brand__logo"><view class="pxi pxi-key"></view></view>
					<view class="ts-brand__meta">
						<text class="ts-brand__en">REWIND</text>
						<text class="ts-brand__cn">倒带重录</text>
					</view>
				</view>

				<text class="ts-title">找回密码</text>
				<text class="ts-lead">验证邮箱后倒带重录你的密码</text>

				<view class="ts-steps">
					<view class="ts-step" :class="{ on: step >= 1 }">
						<view class="ts-step__n">1</view>
						<text>验证邮箱</text>
					</view>
					<view class="ts-step__line" :class="{ on: step >= 2 }"></view>
					<view class="ts-step" :class="{ on: step >= 2 }">
						<view class="ts-step__n">2</view>
						<text>重置密码</text>
					</view>
				</view>

				<view class="ts-form">
					<view class="ts-field">
						<view class="ts-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="ts-field__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#8b939c" maxlength="40" />
						<view class="ts-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="ts-cells">
						<input v-for="(c, i) in cells" :key="i" class="ts-cell" :class="{ dim: !c, on: focusIndex === i }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>

					<view class="ts-field">
						<view class="ts-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="ts-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#8b939c" maxlength="20" />
						<view class="ts-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="ts-field">
						<view class="ts-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="ts-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#8b939c" maxlength="20" />
					</view>

					<view class="ts-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>倒 带 重 录</text>
					</view>

					<view class="ts-foot">
						<text>想起密码了？</text>
						<text class="ts-foot__link" @click="goLogin">返回登录</text>
					</view>
				</view>
			</view>
			<view class="ts-wave">
				<view v-for="n in 24" :key="n" class="ts-wave__b" :class="'ts-wave__b--' + (n % 6)"></view>
			</view>
			<view class="ts-keys">
				<view class="ts-key">&lt;&lt;</view>
				<view class="ts-key ts-key--play">&gt;</view>
				<view class="ts-key">[]</view>
				<view class="ts-key">&gt;&gt;</view>
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
