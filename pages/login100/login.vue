<template>
	<view class="sp-page">
		<view class="sp-nav">
			<view class="sp-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="sp-nav__title">登 录</text>
			<text class="sp-nav__slot">ORBIT</text>
		</view>

		<view class="sp-body">
			<view class="sp-radar">
				<view class="sp-radar__ring"></view>
				<view class="sp-radar__ring sp-radar__ring--inner"></view>
				<view class="sp-radar__cross"></view>
				<view class="sp-radar__sweep"></view>
				<view class="sp-radar__blip sp-radar__blip--a"></view>
				<view class="sp-radar__blip sp-radar__blip--b"></view>
			</view>
			<view class="sp-card">
				<view class="sp-brand">
					<view class="sp-brand__logo"><view class="pxi pxi-chip"></view></view>
					<view class="sp-brand__meta">
						<text class="sp-brand__en">CREW ID</text>
						<text class="sp-brand__cn">船员身份</text>
					</view>
				</view>

				<text class="sp-title">登 录</text>
				<text class="sp-lead">验证身份后接入主控系统</text>

				<view class="sp-tabs">
					<view v-for="(item, i) in tabs" :key="i" class="sp-tab" :class="{ on: tab === i }" @click="tab = i">{{ item }}</view>
				</view>

				<view class="sp-form">
					<block v-if="tab === 0">
						<view class="sp-field">
							<view class="sp-field__ico"><view class="pxi pxi-user"></view></view>
							<input class="sp-field__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#3f7a63" maxlength="40" />
						</view>
						<view class="sp-field">
							<view class="sp-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="sp-field__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#3f7a63" maxlength="20" />
							<view class="sp-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
						<view class="sp-row">
							<text class="sp-row__link" @click="goForget">忘记密码？</text>
						</view>
					</block>
					<block v-else-if="tab === 1">
						<view class="sp-field">
							<view class="sp-field__ico"><view class="pxi pxi-phone"></view></view>
							<input class="sp-field__in sp-field__in--en" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#3f7a63" maxlength="11" />
						</view>
						<view class="sp-field">
							<view class="sp-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="sp-field__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#3f7a63" maxlength="6" />
							<view class="sp-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>
					<block v-else>
						<view class="sp-field">
							<view class="sp-field__ico"><view class="pxi pxi-mail"></view></view>
							<input class="sp-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#3f7a63" maxlength="40" />
						</view>
						<view class="sp-field">
							<view class="sp-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="sp-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#3f7a63" maxlength="6" />
							<view class="sp-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>

					<view class="sp-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>接 入 主 控</text>
					</view>

					<view class="sp-foot">
						<text>尚无船员编号？</text>
						<text class="sp-foot__link" @click="goRegister">立即注册</text>
					</view>

					<view class="sp-agree" @click="agreed = !agreed">
						<view class="sp-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="sp-link">《用户协议》</text>与<text class="sp-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="sp-hud">
				<view class="sp-hud__col">
					<text class="sp-hud__l">SECTOR 07</text>
					<text class="sp-hud__l">BEARING 214°</text>
				</view>
				<view class="sp-hud__col sp-hud__col--r">
					<text class="sp-hud__l">X 128.44</text>
					<text class="sp-hud__l">Y 902.10</text>
				</view>
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
				uni.navigateTo({ url: '/pages/login100/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login100/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
