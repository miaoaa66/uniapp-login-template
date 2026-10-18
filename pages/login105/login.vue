<template>
	<view class="vd-page">
		<view class="vd-nav">
			<view class="vd-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="vd-nav__title">登 录</text>
			<text class="vd-nav__slot">A1</text>
		</view>

		<view class="vd-body">
			<view class="vd-light">
				<text class="vd-light__t">DRINKS 24H</text>
				<view class="vd-light__bars">
					<view v-for="n in 4" :key="n" class="vd-light__bar" :class="'vd-light__bar--' + (n % 2)"></view>
				</view>
			</view>
			<view class="vd-price">
				<text class="vd-price__l">ITEM A1</text>
				<text class="vd-price__r">¥150</text>
			</view>
			<view class="vd-card">
				<view class="vd-brand">
					<view class="vd-brand__logo"><view class="pxi pxi-invader"></view></view>
					<view class="vd-brand__meta">
						<text class="vd-brand__en">VENDING ID</text>
						<text class="vd-brand__cn">售货机会员</text>
					</view>
				</view>

				<text class="vd-title">登 录</text>
				<text class="vd-lead">投币口就在下方，来一罐冰镇的今天</text>

				<view class="vd-tabs">
					<view v-for="(item, i) in tabs" :key="i" class="vd-tab" :class="{ on: tab === i }" @click="tab = i">{{ item }}</view>
				</view>

				<view class="vd-form">
					<block v-if="tab === 0">
						<view class="vd-field">
							<view class="vd-field__ico"><view class="pxi pxi-user"></view></view>
							<input class="vd-field__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#8b97a8" maxlength="40" />
						</view>
						<view class="vd-field">
							<view class="vd-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="vd-field__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#8b97a8" maxlength="20" />
							<view class="vd-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
						<view class="vd-row">
							<text class="vd-row__link" @click="goForget">忘记密码？</text>
						</view>
					</block>
					<block v-else-if="tab === 1">
						<view class="vd-field">
							<view class="vd-field__ico"><view class="pxi pxi-phone"></view></view>
							<input class="vd-field__in vd-field__in--en" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#8b97a8" maxlength="11" />
						</view>
						<view class="vd-field">
							<view class="vd-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="vd-field__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#8b97a8" maxlength="6" />
							<view class="vd-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>
					<block v-else>
						<view class="vd-field">
							<view class="vd-field__ico"><view class="pxi pxi-mail"></view></view>
							<input class="vd-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#8b97a8" maxlength="40" />
						</view>
						<view class="vd-field">
							<view class="vd-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="vd-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#8b97a8" maxlength="6" />
							<view class="vd-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>

					<view class="vd-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>投 币 购 买</text>
					</view>

					<view class="vd-foot">
						<text>口袋里没有硬币？</text>
						<text class="vd-foot__link" @click="goRegister">立即注册</text>
					</view>

					<view class="vd-agree" @click="agreed = !agreed">
						<view class="vd-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="vd-link">《用户协议》</text>与<text class="vd-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="vd-shelf">
				<view v-for="n in 6" :key="n" class="vd-slot">
					<view class="vd-can" :class="'vd-can--' + (n % 3)"></view>
					<text class="vd-slot__no">{{ 'A' + n }}</text>
				</view>
			</view>
			<view class="vd-tray">
				<view class="vd-tray__mouth"></view>
				<text class="vd-tray__t">PUSH</text>
				<view class="vd-tray__arrow"><view class="pxi pxi-arrow"></view></view>
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
				this.toast('出货成功，请取走罐子')
			},

			goBack() {
				uni.navigateBack()
			},

			goRegister() {
				uni.navigateTo({ url: '/pages/login105/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login105/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
