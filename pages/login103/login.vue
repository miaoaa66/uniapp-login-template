<template>
	<view class="ca-page">
		<view class="ca-nav">
			<view class="ca-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="ca-nav__title">登 录</text>
			<text class="ca-nav__slot">14</text>
		</view>

		<view class="ca-body">
			<view class="ca-month">
				<text class="ca-month__m">OCT</text>
				<text class="ca-month__y">2026</text>
			</view>
			<view class="ca-card">
				<view class="ca-brand">
					<view class="ca-brand__logo"><view class="pxi pxi-chip"></view></view>
					<view class="ca-brand__meta">
						<text class="ca-brand__en">CALENDAR</text>
						<text class="ca-brand__cn">打卡账户</text>
					</view>
				</view>

				<text class="ca-title">登 录</text>
				<text class="ca-lead">登录后开始今天的打卡</text>

				<view class="ca-tabs">
					<view v-for="(item, i) in tabs" :key="i" class="ca-tab" :class="{ on: tab === i }" @click="tab = i">{{ item }}</view>
				</view>

				<view class="ca-form">
					<block v-if="tab === 0">
						<view class="ca-field">
							<view class="ca-field__ico"><view class="pxi pxi-user"></view></view>
							<input class="ca-field__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#9aa3af" maxlength="40" />
						</view>
						<view class="ca-field">
							<view class="ca-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="ca-field__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#9aa3af" maxlength="20" />
							<view class="ca-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
						<view class="ca-row">
							<text class="ca-row__link" @click="goForget">忘记密码？</text>
						</view>
					</block>
					<block v-else-if="tab === 1">
						<view class="ca-field">
							<view class="ca-field__ico"><view class="pxi pxi-phone"></view></view>
							<input class="ca-field__in ca-field__in--en" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#9aa3af" maxlength="11" />
						</view>
						<view class="ca-field">
							<view class="ca-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="ca-field__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#9aa3af" maxlength="6" />
							<view class="ca-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>
					<block v-else>
						<view class="ca-field">
							<view class="ca-field__ico"><view class="pxi pxi-mail"></view></view>
							<input class="ca-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#9aa3af" maxlength="40" />
						</view>
						<view class="ca-field">
							<view class="ca-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="ca-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#9aa3af" maxlength="6" />
							<view class="ca-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>

					<view class="ca-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>今 日 打 卡</text>
					</view>

					<view class="ca-foot">
						<text>还没有账户？</text>
						<text class="ca-foot__link" @click="goRegister">立即注册</text>
					</view>

					<view class="ca-agree" @click="agreed = !agreed">
						<view class="ca-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="ca-link">《用户协议》</text>与<text class="ca-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="ca-week">
				<text v-for="(w, i) in week" :key="i" class="ca-week__d">{{ w }}</text>
			</view>
			<view class="ca-grid">
				<view v-for="n in 28" :key="n" class="ca-day"
					:class="{ 'ca-day--today': n === 14, 'ca-day--past': n < 14, 'ca-day--off': n > 21 }">{{ n }}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabs: ["密码登录","手机验证码","邮箱验证码"],
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
				timer: null,
				week: ["日","一","二","三","四","五","六"]
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
				uni.showToast({ title, icon: 'none' })
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
				uni.navigateTo({ url: '/pages/login103/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login103/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
