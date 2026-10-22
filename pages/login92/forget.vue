<template>
	<view class="fc-page">
		<view class="fc-nav">
			<view class="fc-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="fc-nav__title">CHANNEL-03 · RESET</text>
			<text class="fc-nav__slot">1983</text>
		</view>

		<!-- 电视机 -->
		<view class="fc-tv">
			<!-- CRT 屏幕：CONTINUE? -->
			<view class="fc-screen fc-screen--form">
				<view class="fc-in">
					<text class="fc-cont">CONTINUE?</text>
					<text class="fc-count">{{ cd }}</text>
					<text class="fc-cont__sub">通过邮箱验证码重设登录密码</text>

					<!-- STEP 1：邮箱 + 验证码 -->
					<view class="fc-sec">
						<view class="fc-cursor"></view>
						<text>STEP 1 · 邮箱验证</text>
					</view>
					<view class="fc-rows">
						<view class="fc-row" :class="{ focus: focusField === 'email' }">
							<view class="fc-row__cur"></view>
							<input class="fc-row__in" v-model="email" placeholder="注册时使用的邮箱"
								placeholder-style="color:#6f7480" maxlength="40"
								@focus="focusField = 'email'" @blur="focusField = ''" />
							<view class="fc-send" :class="{ off: mailLeft > 0 }" @click="sendCode">
								{{ mailLeft > 0 ? mailLeft + 's' : 'SEND' }}
							</view>
						</view>
					</view>

					<!-- 锁位方块：6 格验证码 -->
					<view class="fc-lock">
						<input v-for="(c, i) in cells" :key="i" class="fc-lock__c"
							:class="{ dim: !c, on: focusIndex === i }" v-model="cells[i]" type="number"
							maxlength="1" :focus="focusIndex === i" placeholder-style="color:#3d4350"
							@input="onCell(i, $event)" @focus="focusIndex = i" />
					</view>

					<!-- STEP 2：新密码 -->
					<view class="fc-sec">
						<view class="fc-cursor"></view>
						<text>STEP 2 · 新密码</text>
					</view>
					<view class="fc-rows">
						<view class="fc-row" :class="{ focus: focusField === 'password' }">
							<view class="fc-row__cur"></view>
							<input class="fc-row__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）"
								placeholder-style="color:#6f7480" maxlength="20"
								@focus="focusField = 'password'" @blur="focusField = ''" />
							<view class="fc-row__eye" @click="showPwd = !showPwd">
								<view class="pxi pxi-eye"></view>
							</view>
						</view>
						<view class="fc-row" :class="{ focus: focusField === 'password2' }">
							<view class="fc-row__cur"></view>
							<input class="fc-row__in" v-model="password2" :password="!showPwd" placeholder="确认新密码"
								placeholder-style="color:#6f7480" maxlength="20"
								@focus="focusField = 'password2'" @blur="focusField = ''" />
						</view>
					</view>

					<view class="fc-foot">
						<text>想起密码了？</text>
						<text class="fc-foot__lk" @click="goLogin">返回登录</text>
					</view>
				</view>
			</view>

			<!-- 机身 -->
			<view class="fc-panel">
				<view class="fc-panel__brand">
					<view class="fc-led"></view>
					<text class="fc-panel__t">RESET KEY · RETRY</text>
				</view>
				<view class="fc-panel__mid">
					<view class="fc-dpad"></view>
					<view class="fc-vents">
						<view v-for="n in 4" :key="n" class="fc-vents__i"></view>
					</view>
					<view class="fc-start" @click="submit"><text>START</text></view>
					<view class="fc-ab">
						<view class="fc-rbtn">A</view>
						<view class="fc-rbtn">B</view>
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
				focusField: '',
				cd: 9,
				cdTimer: null,
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
			// 纯装饰：9→0 循环倒计时
			this.cdTimer = setInterval(() => {
				this.cd = this.cd > 0 ? this.cd - 1 : 9
			}, 1000)
		},
		onUnload() {
			this.clearTimer()
			if (this.cdTimer) {
				clearInterval(this.cdTimer)
				this.cdTimer = null
			}
		},
		methods: {
			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
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
