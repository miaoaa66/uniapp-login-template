<template>
	<view class="tk-page">
		<view class="tk-top">
			<view class="tk-back" @click="goBack"><view class="pxi pxi-back"></view></view>
			<text class="tk-top__t">补票窗口</text>
		</view>

		<!-- 排队号小票 -->
		<view class="tk-queue">
			<text class="tk-queue__no">NO.007</text>
			<text class="tk-queue__date">2026-10-22 · 补票专用窗口</text>
		</view>

		<!-- 两步表单：第一联 / 第二联 -->
		<view class="tk-ticket tk-ticket--col">
			<view class="tk-main">
				<view class="tk-part">
					<text class="tk-part__t">{{ step === 1 ? '第一联 · 身份核验' : '第二联 · 重设密码' }}</text>
				</view>

				<block v-if="step === 1">
					<view class="tk-field">
						<text class="tk-field__label">邮箱</text>
						<input class="tk-field__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#93a3b8" maxlength="40" />
						<text class="tk-get" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</text>
					</view>
					<!-- 六个票格验证码 -->
					<view class="tk-cells">
						<input v-for="(c, i) in cells" :key="i" class="tk-cell" :class="{ dim: !c }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>
				</block>
				<block v-else>
					<view class="tk-field">
						<text class="tk-field__label">新密码</text>
						<input class="tk-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#93a3b8" maxlength="20" />
						<view class="tk-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>
					<view class="tk-field">
						<text class="tk-field__label">复述</text>
						<input class="tk-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#93a3b8" maxlength="20" />
					</view>
				</block>

				<view class="tk-btn" @click="next">
					<text class="tk-btn__t">{{ step === 1 ? '确认 · 下一联' : '确认补票' }}</text>
				</view>

				<view class="tk-foot">
					<text class="tk-foot__link" @click="goLogin">想起密码了？返回登录</text>
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

			next() {
				if (this.step === 1) {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
					this.step = 2
				} else {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
					if (this.password.length < 8) return this.toast('新密码至少 8 位')
					if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
					// TODO: 调用后端重置密码接口
					this.toast('密码已重置')
				}
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
