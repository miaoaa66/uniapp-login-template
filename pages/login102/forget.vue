<template>
	<view class="ts-page">
		<view class="ts-device">
			<view class="ts-screw ts-screw--lt"></view>
			<view class="ts-screw ts-screw--rt"></view>
			<view class="ts-screw ts-screw--lb"></view>
			<view class="ts-screw ts-screw--rb"></view>

			<view class="ts-toprow">
				<view class="ts-back" @click="goBack">
					<view class="pxi pxi-back"></view>
				</view>
				<text class="ts-brandt">PIXELMAN · 找回密码</text>
				<view class="ts-led"></view>
			</view>

			<view class="ts-bay">
				<view class="ts-win">
					<view class="ts-reel ts-reel--l"></view>
					<view class="ts-eq">
						<view class="ts-eq__b ts-eq__b--0"></view>
						<view class="ts-eq__b ts-eq__b--1"></view>
						<view class="ts-eq__b ts-eq__b--2"></view>
						<view class="ts-eq__b ts-eq__b--3"></view>
						<view class="ts-eq__b ts-eq__b--4"></view>
					</view>
					<view class="ts-reel ts-reel--r"></view>
				</view>
				<view class="ts-winlabel ts-winlabel--steps">
					<view class="ts-lamp" :class="{ on: step >= 1 }">
						<view class="ts-lamp__dot"></view>
						<text class="ts-lamp__t">STEP 1</text>
					</view>
					<text class="ts-winlabel__t">REW ◀◀ REWIND</text>
					<view class="ts-lamp" :class="{ on: step >= 2 }">
						<view class="ts-lamp__dot"></view>
						<text class="ts-lamp__t">STEP 2</text>
					</view>
				</view>
			</view>

			<view class="ts-list">
				<view class="ts-track">
					<text class="ts-track__no">TRACK 01</text>
					<text class="ts-track__name">注册邮箱</text>
					<input class="ts-track__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#8b939c" maxlength="40" />
					<view class="ts-rec" :class="{ off: mailLeft > 0 }" @click="sendCode">
						<view class="ts-rec__dot"></view>
						<text class="ts-rec__t">{{ mailLeft > 0 ? mailLeft + 's' : 'REC' }}</text>
					</view>
				</view>

				<view class="ts-cells">
					<input v-for="(c, i) in cells" :key="i" class="ts-cell" :class="{ dim: !c, on: focusIndex === i }"
						v-model="cells[i]" type="number" maxlength="1" placeholder-style="color:#565c63" :focus="focusIndex === i" @input="onCell(i, $event)" />
				</view>

				<view class="ts-track">
					<text class="ts-track__no">TRACK 02</text>
					<text class="ts-track__name">新密码</text>
					<input class="ts-track__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#8b939c" maxlength="20" />
					<view class="ts-hold" :class="{ on: showPwd }" @click="showPwd = !showPwd"><text class="ts-hold__t">HOLD</text></view>
				</view>

				<view class="ts-track">
					<text class="ts-track__no">TRACK 03</text>
					<text class="ts-track__name">再确认</text>
					<input class="ts-track__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#8b939c" maxlength="20" />
				</view>

				<view class="ts-keyrow">
					<view class="ts-keycol">
						<view class="ts-mini"><text class="ts-mini__t" @click="goLogin">想起密码了？返回登录 →</text></view>
					</view>
					<view class="ts-play" @click="submit">
						<text class="ts-play__t">重 置</text>
					</view>
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
