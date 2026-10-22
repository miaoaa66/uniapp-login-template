<template>
	<view class="fc-page">
		<!-- 自绘导航（电视机顶信道条） -->
		<view class="fc-nav">
			<view class="fc-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="fc-nav__title">CHANNEL-01 · LOGIN</text>
			<text class="fc-nav__slot">1983</text>
		</view>

		<!-- 电视机 -->
		<view class="fc-tv">
			<!-- CRT 屏幕 -->
			<view class="fc-screen" :class="{ 'fc-screen--form': phase === 'form' }" @click="pressStart">
				<!-- 待机：PRESS START -->
				<block v-if="phase === 'boot'">
					<text class="fc-press">PRESS START</text>
					<text class="fc-press__sub">- TAP SCREEN -</text>
				</block>

				<!-- 开机后：屏内系统 -->
				<block v-else>
					<view class="fc-in">
						<view class="fc-bootlog">
							<text class="fc-bootlog__l fc-bootlog__l--sys">PIXEL-ID SYSTEM</text>
							<text class="fc-bootlog__l fc-bootlog__l--ok">BOOT OK</text>
						</view>

						<!-- 游戏菜单：登录方式 -->
						<view class="fc-menu">
							<view v-for="(t, i) in tabs" :key="i" class="fc-menu__i" :class="{ on: tab === i }"
								@click.stop="tab = i">
								<view class="fc-cursor"></view>
								<text>{{ t }}</text>
							</view>
						</view>

						<!-- 屏内嵌行表单 -->
						<view class="fc-rows">
							<!-- 密码登录 -->
							<block v-if="tab === 0">
								<view class="fc-row" :class="{ focus: focusField === 'account' }">
									<view class="fc-row__cur"></view>
									<input class="fc-row__in" v-model="account" placeholder="手机号 / 邮箱"
										placeholder-style="color:#6f7480" maxlength="40"
										@focus="focusField = 'account'" @blur="focusField = ''" />
								</view>
								<view class="fc-row" :class="{ focus: focusField === 'password' }">
									<view class="fc-row__cur"></view>
									<input class="fc-row__in" v-model="password" :password="!showPwd"
										placeholder="登录密码" placeholder-style="color:#6f7480" maxlength="20"
										@focus="focusField = 'password'" @blur="focusField = ''" />
									<view class="fc-row__eye" @click.stop="showPwd = !showPwd">
										<view class="pxi pxi-eye"></view>
									</view>
								</view>
								<view class="fc-row fc-row--link" @click.stop="goForget">
									<view class="fc-row__cur"></view>
									<text class="fc-row__lt">忘记密码？</text>
								</view>
							</block>

							<!-- 手机验证码登录 -->
							<block v-else-if="tab === 1">
								<view class="fc-row" :class="{ focus: focusField === 'phone' }">
									<view class="fc-row__cur"></view>
									<input class="fc-row__in fc-row__in--en" v-model="phone" type="number"
										placeholder="+86" placeholder-style="color:#6f7480" maxlength="11"
										@focus="focusField = 'phone'" @blur="focusField = ''" />
								</view>
								<view class="fc-row" :class="{ focus: focusField === 'smsCode' }">
									<view class="fc-row__cur"></view>
									<input class="fc-row__in" v-model="smsCode" type="number" placeholder="短信验证码"
										placeholder-style="color:#6f7480" maxlength="6"
										@focus="focusField = 'smsCode'" @blur="focusField = ''" />
									<view class="fc-send" :class="{ off: smsLeft > 0 }" @click.stop="sendCode('sms')">
										{{ smsLeft > 0 ? smsLeft + 's' : 'SEND' }}
									</view>
								</view>
							</block>

							<!-- 邮箱验证码登录 -->
							<block v-else>
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
									<view class="fc-send" :class="{ off: mailLeft > 0 }" @click.stop="sendCode('mail')">
										{{ mailLeft > 0 ? mailLeft + 's' : 'SEND' }}
									</view>
								</view>
							</block>
						</view>

						<!-- 协议 -->
						<view class="fc-agree" @click.stop="agreed = !agreed">
							<view class="fc-check" :class="{ on: agreed }"></view>
							<text>我已阅读并同意<text class="fc-agree__lk">《用户协议》</text>与<text class="fc-agree__lk">《隐私政策》</text></text>
						</view>

						<view class="fc-foot">
							<text>还没有通行证？</text>
							<text class="fc-foot__lk" @click.stop="goRegister">立即注册</text>
						</view>
					</view>
				</block>
			</view>

			<!-- 机身 -->
			<view class="fc-panel">
				<view class="fc-panel__brand">
					<view class="fc-led"></view>
					<text class="fc-panel__t">PIXEL-1 ENTERTAINMENT SYSTEM</text>
				</view>
				<view class="fc-panel__mid">
					<view class="fc-dpad"></view>
					<view class="fc-vents">
						<view v-for="n in 4" :key="n" class="fc-vents__i"></view>
					</view>
					<view class="fc-start" @click="onStart"><text>START</text></view>
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
				phase: 'boot', // boot | form
				focusField: '',
				tabs: ['密码登录', '手机验证码', '邮箱验证码'],
				tab: 0,
				agreed: false,
				showPwd: false,
				account: '',
				password: '',
				phone: '',
				smsCode: '',
				email: '',
				mailCode: '',
				smsLeft: 0,
				mailLeft: 0,
				timer: null
			}
		},
		onLoad(options) {
			// 演示参数：?tab=1&demo=1
			if (options && options.tab) this.tab = Number(options.tab) || 0
			if (options && options.demo) {
				this.fillDemo()
				this.phase = 'form'
			}
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			// 电视开机：白闪 → 系统卷轴下拉
			pressStart() {
				if (this.phase !== 'boot') return
				this.phase = 'form'
			},

			// 机身 START 键：待机时开机，开机后提交
			onStart() {
				if (this.phase === 'boot') {
					this.pressStart()
				} else {
					this.submit()
				}
			},

			fillDemo() {
				this.agreed = true
				this.account = 'player@pixel.id'
				this.password = '1up-1983'
				this.phone = '13800001234'
				this.email = 'player@pixel.id'
				this.smsCode = '1983'
				this.mailCode = '1983'
			},

			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			isPhone(v) {
				return /^1\d{10}$/.test(v)
			},

			isEmail(v) {
				return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v)
			},

			sendCode(type) {
				if (type === 'sms') {
					if (this.smsLeft > 0) return
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
					this.smsLeft = 60
				} else {
					if (this.mailLeft > 0) return
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					this.mailLeft = 60
				}
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发送')
				this.startTimer()
			},

			startTimer() {
				if (this.timer) return
				this.timer = setInterval(() => {
					if (this.smsLeft > 0) this.smsLeft--
					if (this.mailLeft > 0) this.mailLeft--
					if (this.smsLeft <= 0 && this.mailLeft <= 0) this.clearTimer()
				}, 1000)
			},

			clearTimer() {
				if (this.timer) {
					clearInterval(this.timer)
					this.timer = null
				}
			},

			submit() {
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				if (this.tab === 0) {
					if (!this.account) return this.toast('请输入账号')
					if (!this.password) return this.toast('请输入登录密码')
				} else if (this.tab === 1) {
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
					if (!this.smsCode) return this.toast('请输入短信验证码')
				} else {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (!this.mailCode) return this.toast('请输入邮箱验证码')
				}
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goBack() {
				uni.navigateBack()
			},

			goRegister() {
				uni.navigateTo({
					url: '/pages/login92/register'
				})
			},

			goForget() {
				uni.navigateTo({
					url: '/pages/login92/forget'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
