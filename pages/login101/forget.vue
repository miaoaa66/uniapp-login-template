<template>
	<view class="nt-page">
		<view class="nt-boardpin nt-boardpin--lt"></view>
		<view class="nt-boardpin nt-boardpin--rt"></view>
		<view class="nt-boardpin nt-boardpin--lb"></view>
		<view class="nt-boardpin nt-boardpin--rb"></view>

		<view class="nt-tag" @click="goBack">
			<view class="nt-tag__pin"></view>
			<view class="pxi pxi-back"></view>
			<text class="nt-tag__t">任务板 · 找回密码</text>
		</view>

		<view class="nt-memo">
			<view class="nt-pin"></view>
			<text class="nt-memo__title">备忘 · 找回密码</text>
			<view class="nt-memo__step">
				<text class="nt-memo__stept">{{ step >= 2 ? '第 2 步 · 重写密码' : '第 1 步 · 核对邮箱' }}</text>
			</view>

			<view class="nt-line">
				<text class="nt-line__k">邮箱</text>
				<input class="nt-line__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#8a6a4f" maxlength="40" />
				<text class="nt-send" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '取验证码' }}</text>
			</view>

			<view class="nt-cells">
				<input v-for="(c, i) in cells" :key="i" class="nt-cell" :class="{ dim: !c, on: focusIndex === i }"
					v-model="cells[i]" type="number" maxlength="1" placeholder-style="color:#8a6a4f" :focus="focusIndex === i" @input="onCell(i, $event)" />
			</view>

			<view class="nt-line">
				<text class="nt-line__k">新密码</text>
				<input class="nt-line__in" v-model="password" :password="!showPwd" placeholder="8-20 位" placeholder-style="color:#8a6a4f" maxlength="20" />
				<view class="nt-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
			</view>

			<view class="nt-line">
				<text class="nt-line__k">再一遍</text>
				<input class="nt-line__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#8a6a4f" maxlength="20" />
			</view>

			<view class="nt-tapebtn nt-tapebtn--ink" @click="submit">
				<text class="nt-tapebtn__t">办 完 这 件 事</text>
			</view>

			<view class="nt-mini"><text class="nt-mini__t" @click="goLogin">想起密码了？返回登录 →</text></view>

			<view v-if="done" class="nt-donestamp">
				<text class="nt-donestamp__t">已 办</text>
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
				done: false
			}
		},
		computed: {
			codeFull() {
				return this.cells.join('').length >= 6
			}
		},
		watch: {
			codeFull(v) {
				if (v && this.step < 2) this.step = 2
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
				this.done = true
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
