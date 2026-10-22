<template>
	<view class="tw-page">
		<!-- 补办公函信纸 -->
		<view class="tw-paper">
			<view class="tw-clip" @click="goBack"><view class="tw-clip__inner"></view></view>

			<view class="tw-head">
				<view class="tw-head__line"></view>
				<view class="tw-head__mid">
					<text class="tw-head__en">REISSUE LETTER</text>
					<text class="tw-head__cn">补 办 公 函</text>
				</view>
				<view class="tw-head__line"></view>
			</view>

			<!-- 一、身份核验 -->
			<view class="tw-sec">
				<text class="tw-sec__t">一、身份核验</text>
				<view class="tw-line">
					<text class="tw-line__label">邮箱</text>
					<view class="tw-line__blank">
						<input class="tw-line__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#b7b0a1" maxlength="40" />
						<text class="tw-word" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</text>
					</view>
				</view>
				<view class="tw-cells">
					<input v-for="(c, i) in cells" :key="i" class="tw-cell" :class="{ dim: !c, on: focusIndex === i }"
						v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
				</view>
			</view>

			<!-- 二、重设密码（核验通过后展开） -->
			<view class="tw-sec" v-if="step >= 2">
				<text class="tw-sec__t">二、重设密码</text>
				<view class="tw-line">
					<text class="tw-line__label">新密码</text>
					<view class="tw-line__blank">
						<input class="tw-line__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#b7b0a1" maxlength="20" />
						<view class="tw-line__eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>
				</view>
				<view class="tw-line">
					<text class="tw-line__label">复述</text>
					<view class="tw-line__blank">
						<input class="tw-line__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#b7b0a1" maxlength="20" />
					</view>
				</view>
			</view>

			<view class="tw-post">
				<text class="tw-post__zh">此致</text>
				<text class="tw-post__jing">敬礼！</text>
				<text class="tw-post__date">PIXEL TYPEWRITER CO. · 1983</text>
			</view>

			<view class="tw-keywrap">
				<view class="tw-ribbon" :class="{ go: ribbon }"></view>
				<view class="tw-key" @click="onSend"><text class="tw-key__t">寄 出</text></view>
			</view>

			<view class="tw-foot">
				<text class="tw-foot__link" @click="goLogin">想起密码了？返回登录</text>
			</view>

			<!-- 存档成功盖章 -->
			<view class="tw-stamp" v-if="stamped"><text class="tw-stamp__t">存档成功</text></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				step: 1,
				stamped: false,
				ribbon: false,
				ribbonTimer: null,
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
			if (this.ribbonTimer) clearTimeout(this.ribbonTimer)
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

			fireRibbon() {
				this.ribbon = false
				if (this.ribbonTimer) clearTimeout(this.ribbonTimer)
				setTimeout(() => { this.ribbon = true }, 30)
				this.ribbonTimer = setTimeout(() => { this.ribbon = false }, 800)
			},

			onSend() {
				this.fireRibbon()
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
					this.stamped = true
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
