<template>
	<view class="fc-page">
		<!-- 自绘导航栏 -->
		<view class="fc-nav">
			<view class="fc-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="fc-nav__title">登 录</text>
			<text class="fc-nav__slot">1983</text>
		</view>

		<view class="fc-body">
			<!-- 机身铭牌 -->
			<view class="fc-plate">
				<view class="fc-led"></view>
				<text class="fc-plate__t">PIXEL-1 ENTERTAINMENT SYSTEM</text>
				<text class="fc-plate__tag">POWER</text>
			</view>

			<view class="fc-card">
				<view class="fc-brand">
					<view class="fc-brand__logo">
						<view class="pxi pxi-invader"></view>
					</view>
					<view class="fc-brand__meta">
						<text class="fc-brand__en">PIXEL ID</text>
						<text class="fc-brand__cn">像素通行证</text>
					</view>
				</view>

				<text class="fc-title">登 录</text>
				<text class="fc-lead">插入卡带，继续你的像素冒险</text>

				<!-- 登录方式切换 -->
				<view class="fc-tabs">
					<view v-for="(t, i) in tabs" :key="i" class="fc-tab" :class="{ on: tab === i }" @click="tab = i">
						{{ t }}
					</view>
				</view>

				<view class="fc-form">
					<!-- 密码登录 -->
					<block v-if="tab === 0">
						<view class="fc-field">
							<view class="fc-field__ico"><view class="pxi pxi-user"></view></view>
							<input class="fc-field__in" v-model="account" placeholder="手机号 / 邮箱"
								placeholder-style="color:#a8abb4" maxlength="40" />
						</view>
						<view class="fc-field">
							<view class="fc-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="fc-field__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码"
								placeholder-style="color:#a8abb4" maxlength="20" />
							<view class="fc-eye" @click="showPwd = !showPwd">
								<view class="pxi pxi-eye"></view>
							</view>
						</view>
						<view class="fc-row">
							<text class="fc-row__link" @click="goForget">忘记密码？</text>
						</view>
					</block>

					<!-- 手机验证码登录 -->
					<block v-else-if="tab === 1">
						<view class="fc-field">
							<view class="fc-field__ico"><view class="pxi pxi-phone"></view></view>
							<input class="fc-field__in fc-field__in--en" v-model="phone" type="number" placeholder="+86"
								placeholder-style="color:#a8abb4" maxlength="11" />
						</view>
						<view class="fc-field">
							<view class="fc-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="fc-field__in" v-model="smsCode" type="number" placeholder="短信验证码"
								placeholder-style="color:#a8abb4" maxlength="6" />
							<view class="fc-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">
								{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}
							</view>
						</view>
					</block>

					<!-- 邮箱验证码登录 -->
					<block v-else>
						<view class="fc-field">
							<view class="fc-field__ico"><view class="pxi pxi-mail"></view></view>
							<input class="fc-field__in" v-model="email" placeholder="player@pixel.id"
								placeholder-style="color:#a8abb4" maxlength="40" />
						</view>
						<view class="fc-field">
							<view class="fc-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="fc-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码"
								placeholder-style="color:#a8abb4" maxlength="6" />
							<view class="fc-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">
								{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}
							</view>
						</view>
					</block>

					<view class="fc-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>登 录</text>
					</view>

					<view class="fc-foot">
						<text>还没有通行证？</text>
						<text class="fc-foot__link" @click="goRegister">立即注册</text>
					</view>

					<view class="fc-agree" @click="agreed = !agreed">
						<view class="fc-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="fc-link">《用户协议》</text>与<text class="fc-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>

			<!-- 机身装饰 -->
			<view class="fc-deco">
				<view class="fc-carts">
					<view v-for="n in 9" :key="n" class="fc-carts__i"></view>
				</view>
				<view class="fc-pad">
					<view class="fc-dpad"></view>
					<view class="fc-hint">
						<text class="fc-hint__t">START</text>
						<text class="fc-hint__t">SELECT</text>
					</view>
					<view class="fc-ab">
						<view class="fc-rbtn">A</view>
						<view class="fc-rbtn">B</view>
					</view>
				</view>
				<view class="fc-hud">
					<text>© 2026 PIXEL-ID LAB</text>
					<text>1UP 000000</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
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
			if (options && options.demo) this.fillDemo()
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
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
