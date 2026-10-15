<template>
	<view class="am-page">
		<view class="am-nav">
			<view class="am-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="am-nav__title">登 录</text>
			<text class="am-nav__slot">tty0</text>
		</view>

		<view class="am-body">
			<view class="am-term">
				<view class="am-term__bar">
					<view class="am-term__dot"></view>
					<view class="am-term__dot"></view>
					<view class="am-term__dot"></view>
					<text class="am-term__title">AMBER-OS v1.0 / tty0</text>
				</view>
			</view>
			<view class="am-card">
				<view class="am-brand">
					<view class="am-brand__logo"><view class="pxi pxi-chip"></view></view>
					<view class="am-brand__meta">
						<text class="am-brand__en">AMBER ID</text>
						<text class="am-brand__cn">终端账户</text>
					</view>
				</view>

				<text class="am-title">登 录</text>
				<text class="am-lead">输入凭据以接入 AMBER-OS</text>

				<view class="am-tabs">
					<view v-for="(item, i) in tabs" :key="i" class="am-tab" :class="{ on: tab === i }" @click="tab = i">{{ item }}</view>
				</view>

				<view class="am-form">
					<block v-if="tab === 0">
						<view class="am-field">
							<view class="am-field__ico"><view class="pxi pxi-user"></view></view>
							<input class="am-field__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#82600e" maxlength="40" />
						</view>
						<view class="am-field">
							<view class="am-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="am-field__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#82600e" maxlength="20" />
							<view class="am-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
						<view class="am-row">
							<text class="am-row__link" @click="goForget">忘记密码？</text>
						</view>
					</block>
					<block v-else-if="tab === 1">
						<view class="am-field">
							<view class="am-field__ico"><view class="pxi pxi-phone"></view></view>
							<input class="am-field__in am-field__in--en" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#82600e" maxlength="11" />
						</view>
						<view class="am-field">
							<view class="am-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="am-field__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#82600e" maxlength="6" />
							<view class="am-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>
					<block v-else>
						<view class="am-field">
							<view class="am-field__ico"><view class="pxi pxi-mail"></view></view>
							<input class="am-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#82600e" maxlength="40" />
						</view>
						<view class="am-field">
							<view class="am-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="am-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#82600e" maxlength="6" />
							<view class="am-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>

					<view class="am-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>接 入</text>
					</view>

					<view class="am-foot">
						<text>尚无账户？</text>
						<text class="am-foot__link" @click="goRegister">立即注册</text>
					</view>

					<view class="am-agree" @click="agreed = !agreed">
						<view class="am-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="am-link">《用户协议》</text>与<text class="am-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="am-log">
				<text class="am-log__l">&gt; LOADING MODULES ....... [OK]</text>
				<text class="am-log__l">&gt; MOUNT /dev/pixel ........ [OK]</text>
				<text class="am-log__l">&gt; READY<span class="am-caret">_</span></text>
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
				uni.navigateTo({ url: '/pages/login97/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login97/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
