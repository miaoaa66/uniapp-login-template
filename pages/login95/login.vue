<template>
	<view class="ac-page">
		<view class="ac-nav">
			<view class="ac-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="ac-nav__title">登 录</text>
			<text class="ac-nav__slot">CR 1</text>
		</view>

		<view class="ac-body">
			<view class="ac-marquee">
				<text class="ac-marquee__t">A R C A D E</text>
			</view>
			<view class="ac-coin">
				<text class="ac-coin__t">INSERT COIN</text>
				<view class="ac-coin__slot"></view>
				<view class="ac-coin__led"></view>
			</view>
			<view class="ac-card">
				<view class="ac-brand">
					<view class="ac-brand__logo"><view class="pxi pxi-invader"></view></view>
					<view class="ac-brand__meta">
						<text class="ac-brand__en">ARCADE ID</text>
						<text class="ac-brand__cn">街机通行证</text>
					</view>
				</view>

				<text class="ac-title">投 币 开 始</text>
				<text class="ac-lead">选择你的登录方式，按下 START 键继续</text>

				<view class="ac-tabs">
					<view v-for="(item, i) in tabs" :key="i" class="ac-tab" :class="{ on: tab === i }" @click="tab = i">{{ item }}</view>
				</view>

				<view class="ac-form">
					<block v-if="tab === 0">
						<view class="ac-field">
							<view class="ac-field__ico"><view class="pxi pxi-user"></view></view>
							<input class="ac-field__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#8f7cc4" maxlength="40" />
						</view>
						<view class="ac-field">
							<view class="ac-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="ac-field__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#8f7cc4" maxlength="20" />
							<view class="ac-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
						<view class="ac-row">
							<text class="ac-row__link" @click="goForget">忘记密码？</text>
						</view>
					</block>
					<block v-else-if="tab === 1">
						<view class="ac-field">
							<view class="ac-field__ico"><view class="pxi pxi-phone"></view></view>
							<input class="ac-field__in ac-field__in--en" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#8f7cc4" maxlength="11" />
						</view>
						<view class="ac-field">
							<view class="ac-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="ac-field__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#8f7cc4" maxlength="6" />
							<view class="ac-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>
					<block v-else>
						<view class="ac-field">
							<view class="ac-field__ico"><view class="pxi pxi-mail"></view></view>
							<input class="ac-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#8f7cc4" maxlength="40" />
						</view>
						<view class="ac-field">
							<view class="ac-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="ac-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#8f7cc4" maxlength="6" />
							<view class="ac-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>

					<view class="ac-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>START</text>
					</view>

					<view class="ac-foot">
						<text>还没有街机档案？</text>
						<text class="ac-foot__link" @click="goRegister">立即注册</text>
					</view>

					<view class="ac-agree" @click="agreed = !agreed">
						<view class="ac-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="ac-link">《用户协议》</text>与<text class="ac-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="ac-panel">
				<view class="ac-stick"><view class="ac-stick__ball"></view></view>
				<view class="ac-btns">
					<view v-for="n in 6" :key="n" class="ac-key" :class="'ac-key--' + n"></view>
				</view>
			</view>
			<view class="ac-hud">
				<text class="ac-hud__l">1UP 004500</text>
				<text class="ac-hud__l">HIGH 012800</text>
				<text class="ac-hud__l">CREDIT 1</text>
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
				uni.navigateTo({ url: '/pages/login95/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login95/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
