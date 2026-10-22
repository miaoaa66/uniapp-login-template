<template>
	<view class="fc-page">
		<view class="fc-nav">
			<view class="fc-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="fc-nav__title">CHANNEL-02 · REGIST</text>
			<text class="fc-nav__slot">1983</text>
		</view>

		<!-- 电视机 -->
		<view class="fc-tv">
			<!-- CRT 屏幕：PLAYER SELECT -->
			<view class="fc-screen fc-screen--form">
				<view class="fc-in">
					<text class="fc-select">PLAYER SELECT</text>
					<text class="fc-select__sub">创建你的像素通行证</text>

					<!-- 高分榜署名：大字昵称 -->
					<view class="fc-name">
						<text class="fc-name__label">HI-SCORE NAME</text>
						<input class="fc-name__in" v-model="nickname" placeholder="昵称（2-12 字）"
							placeholder-style="color:#5a6070" maxlength="12" />
					</view>

					<!-- 屏内嵌行表单 -->
					<view class="fc-rows">
						<view class="fc-row" :class="{ focus: focusField === 'email' }">
							<view class="fc-row__cur"></view>
							<input class="fc-row__in" v-model="email" placeholder="player@pixel.id"
								placeholder-style="color:#6f7480" maxlength="40"
								@focus="focusField = 'email'" @blur="focusField = ''" />
						</view>
						<view class="fc-row" :class="{ focus: focusField === 'mailCode' }">
							<view class="fc-row__cur"></view>
							<input class="fc-row__in" v-model="mailCode" type="number" placeholder="邮箱验证码"
								placeholder-style="color:#6f7480" maxlength="6"
								@focus="focusField = 'mailCode'" @blur="focusField = ''" />
							<view class="fc-send" :class="{ off: mailLeft > 0 }" @click="sendCode">
								{{ mailLeft > 0 ? mailLeft + 's' : 'SEND' }}
							</view>
						</view>
						<view class="fc-row" :class="{ focus: focusField === 'password' }">
							<view class="fc-row__cur"></view>
							<input class="fc-row__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）"
								placeholder-style="color:#6f7480" maxlength="20"
								@focus="focusField = 'password'" @blur="focusField = ''" />
							<view class="fc-row__eye" @click="showPwd = !showPwd">
								<view class="pxi pxi-eye"></view>
							</view>
						</view>
						<view class="fc-row" :class="{ focus: focusField === 'password2' }">
							<view class="fc-row__cur"></view>
							<input class="fc-row__in" v-model="password2" :password="!showPwd" placeholder="确认密码"
								placeholder-style="color:#6f7480" maxlength="20"
								@focus="focusField = 'password2'" @blur="focusField = ''" />
						</view>
					</view>

					<!-- HP 三格 -->
					<view class="fc-hp">
						<text class="fc-hp__t">HP</text>
						<view class="fc-hp__cells">
							<view class="fc-hp__c" :class="{ on: strength >= 1 }"></view>
							<view class="fc-hp__c" :class="{ on: strength >= 2 }"></view>
							<view class="fc-hp__c" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="fc-hp__v">{{ strengthText }}</text>
					</view>

					<!-- 协议 -->
					<view class="fc-agree" @click="agreed = !agreed">
						<view class="fc-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="fc-agree__lk">《用户协议》</text>与<text class="fc-agree__lk">《隐私政策》</text></text>
					</view>

					<view class="fc-foot">
						<text>已有通行证？</text>
						<text class="fc-foot__lk" @click="goLogin">返回登录</text>
					</view>
				</view>
			</view>

			<!-- 机身 -->
			<view class="fc-panel">
				<view class="fc-panel__brand">
					<view class="fc-led"></view>
					<text class="fc-panel__t">INSERT CARTRIDGE · PLAYER 1</text>
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
				focusField: '',
				agreed: false,
				showPwd: false,
				nickname: '',
				email: '',
				mailCode: '',
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null
			}
		},
		computed: {
			strength() {
				const v = this.password
				if (!v) return 0
				let s = 0
				if (v.length >= 8) s++
				if (/[a-zA-Z]/.test(v) && /\d/.test(v)) s++
				if (/[^a-zA-Z0-9]/.test(v)) s++
				return s
			},
			strengthText() {
				return ['──', '弱', '中', '强'][this.strength]
			}
		},
		onLoad(options) {
			if (options && options.demo) {
				this.agreed = true
				this.nickname = 'PLAYER-1'
				this.email = 'player@pixel.id'
				this.mailCode = '1983'
				this.password = '1up-1983'
				this.password2 = '1up-1983'
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

			isEmail(v) {
				return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v)
			},

			sendCode() {
				if (this.mailLeft > 0) return
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				this.mailLeft = 60
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
				if (!this.nickname) return this.toast('请输入昵称')
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (!this.mailCode) return this.toast('请输入邮箱验证码')
				if (this.password.length < 8) return this.toast('密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端注册接口
				this.toast('注册成功')
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
