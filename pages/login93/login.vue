<template>
	<view class="gb-page">
		<!-- 自绘导航栏 -->
		<view class="gb-nav">
			<view class="gb-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="gb-nav__title">登 录</text>
			<view class="gb-nav__led">
				<view class="gb-led"></view>
			</view>
		</view>

		<view class="gb-body">
			<!-- 掌机外壳 -->
			<view class="gb-shell">
				<view class="gb-shell__brand">
					<text class="gb-shell__en">PIXEL BOY</text>
					<text class="gb-shell__sub">DOT MATRIX STEREO SOUND</text>
				</view>

				<!-- LCD 屏幕 -->
				<view class="gb-screen">
					<view class="gb-head">
						<view class="gb-head__logo">
							<view class="pxi pxi-invader"></view>
						</view>
						<view class="gb-head__meta">
							<text class="gb-head__en">PIXEL ID</text>
							<text class="gb-head__cn">像素通行证</text>
						</view>
					</view>

					<text class="gb-title">登 录</text>
					<text class="gb-lead">按下 START，继续你的存档</text>

					<view class="gb-tabs">
						<view v-for="(t, i) in tabs" :key="i" class="gb-tab" :class="{ on: tab === i }" @click="tab = i">
							{{ t }}
						</view>
					</view>

					<view class="gb-form">
						<!-- 密码登录 -->
						<block v-if="tab === 0">
							<view class="gb-field">
								<view class="gb-field__ico"><view class="pxi pxi-user"></view></view>
								<input class="gb-field__in" v-model="account" placeholder="手机号 / 邮箱"
									placeholder-style="color:#6f8a4a" maxlength="40" />
							</view>
							<view class="gb-field">
								<view class="gb-field__ico"><view class="pxi pxi-lock"></view></view>
								<input class="gb-field__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码"
									placeholder-style="color:#6f8a4a" maxlength="20" />
								<view class="gb-eye" @click="showPwd = !showPwd">
									<view class="pxi pxi-eye"></view>
								</view>
							</view>
							<view class="gb-row">
								<text class="gb-row__link" @click="goForget">忘记密码？</text>
							</view>
						</block>

						<!-- 邮箱验证码登录 -->
						<block v-else-if="tab === 1">
							<view class="gb-field">
								<view class="gb-field__ico"><view class="pxi pxi-mail"></view></view>
								<input class="gb-field__in" v-model="email" placeholder="player@pixel.id"
									placeholder-style="color:#6f8a4a" maxlength="40" />
							</view>
							<view class="gb-field">
								<view class="gb-field__ico"><view class="pxi pxi-shield"></view></view>
								<input class="gb-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码"
									placeholder-style="color:#6f8a4a" maxlength="6" />
								<view class="gb-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">
									{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}
								</view>
							</view>
						</block>

						<!-- 手机验证码登录 -->
						<block v-else>
							<view class="gb-field">
								<view class="gb-field__ico"><view class="pxi pxi-phone"></view></view>
								<input class="gb-field__in gb-field__in--en" v-model="phone" type="number" placeholder="+86"
									placeholder-style="color:#6f8a4a" maxlength="11" />
							</view>
							<view class="gb-field">
								<view class="gb-field__ico"><view class="pxi pxi-shield"></view></view>
								<input class="gb-field__in" v-model="smsCode" type="number" placeholder="短信验证码"
									placeholder-style="color:#6f8a4a" maxlength="6" />
								<view class="gb-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">
									{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}
								</view>
							</view>
						</block>

						<view class="gb-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>START · 登 录</text>
					</view>

						<view class="gb-foot">
							<text>没有存档？</text>
							<text class="gb-foot__link" @click="goRegister">新建存档</text>
						</view>

						<view class="gb-agree" @click="agreed = !agreed">
							<view class="gb-check" :class="{ on: agreed }"></view>
							<text>我已阅读并同意<text class="gb-link">《用户协议》</text>与<text class="gb-link">《隐私政策》</text></text>
						</view>
					</view>
				</view>

				<!-- 底部控制器 -->
				<view class="gb-ctrl">
					<view class="gb-dpad">
						<view class="gb-dpad__h"></view>
						<view class="gb-dpad__v"></view>
						<view class="gb-dpad__c"></view>
					</view>
					<view class="gb-btns">
						<view class="gb-abtn">B</view>
						<view class="gb-abtn">A</view>
					</view>
				</view>

				<view class="gb-speaker">
					<view v-for="n in 5" :key="n" class="gb-speaker__i"></view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabs: ['密码登录', '邮箱验证码', '手机验证码'],
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
			if (options && options.tab) this.tab = Number(options.tab) || 0
			if (options && options.demo) this.fillDemo()
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			fillDemo() {
				this.agreed = true
				this.account = 'player@pixel.id'
				this.password = 'dmG-1989'
				this.phone = '13800001234'
				this.email = 'player@pixel.id'
				this.smsCode = '1989'
				this.mailCode = '1989'
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
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (!this.mailCode) return this.toast('请输入邮箱验证码')
				} else {
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
					if (!this.smsCode) return this.toast('请输入短信验证码')
				}
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goBack() {
				uni.navigateBack()
			},

			goRegister() {
				uni.navigateTo({
					url: '/pages/login93/register'
				})
			},

			goForget() {
				uni.navigateTo({
					url: '/pages/login93/forget'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
