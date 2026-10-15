<template>
	<view class="tw-page">
		<view class="tw-nav">
			<view class="tw-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="tw-nav__title">登 录</text>
			<text class="tw-nav__slot">1983</text>
		</view>

		<view class="tw-body">
			<view class="tw-head">
				<text class="tw-head__no">FORM NO.1983</text>
				<text class="tw-head__t">PIXEL TYPEWRITER CO.</text>
			</view>
			<view class="tw-rule"></view>
			<view class="tw-card">
				<view class="tw-brand">
					<view class="tw-brand__logo"><view class="pxi pxi-chip"></view></view>
					<view class="tw-brand__meta">
						<text class="tw-brand__en">TYPEWRITER</text>
						<text class="tw-brand__cn">打字机账户</text>
					</view>
				</view>

				<text class="tw-title">登 录</text>
				<text class="tw-lead">敲下你的账号与密码，开始排版</text>

				<view class="tw-tabs">
					<view v-for="(item, i) in tabs" :key="i" class="tw-tab" :class="{ on: tab === i }" @click="tab = i">{{ item }}</view>
				</view>

				<view class="tw-form">
					<block v-if="tab === 0">
						<view class="tw-field">
							<view class="tw-field__ico"><view class="pxi pxi-user"></view></view>
							<input class="tw-field__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#a49c8a" maxlength="40" />
						</view>
						<view class="tw-field">
							<view class="tw-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="tw-field__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#a49c8a" maxlength="20" />
							<view class="tw-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
						<view class="tw-row">
							<text class="tw-row__link" @click="goForget">忘记密码？</text>
						</view>
					</block>
					<block v-else-if="tab === 1">
						<view class="tw-field">
							<view class="tw-field__ico"><view class="pxi pxi-phone"></view></view>
							<input class="tw-field__in tw-field__in--en" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#a49c8a" maxlength="11" />
						</view>
						<view class="tw-field">
							<view class="tw-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="tw-field__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#a49c8a" maxlength="6" />
							<view class="tw-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>
					<block v-else>
						<view class="tw-field">
							<view class="tw-field__ico"><view class="pxi pxi-mail"></view></view>
							<input class="tw-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#a49c8a" maxlength="40" />
						</view>
						<view class="tw-field">
							<view class="tw-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="tw-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#a49c8a" maxlength="6" />
							<view class="tw-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>

					<view class="tw-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>敲 下 回 车</text>
					</view>

					<view class="tw-foot">
						<text>还没有账户？</text>
						<text class="tw-foot__link" @click="goRegister">立即注册</text>
					</view>

					<view class="tw-agree" @click="agreed = !agreed">
						<view class="tw-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="tw-link">《用户协议》</text>与<text class="tw-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="tw-keys">
				<view v-for="n in 3" :key="n" class="tw-keys__row">
					<view v-for="m in 8" :key="m" class="tw-key"></view>
				</view>
			</view>
			<view class="tw-platen"></view>
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
				uni.navigateTo({ url: '/pages/login98/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login98/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
