<template>
	<view class="rq-page">
		<view class="rq-nav">
			<view class="rq-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="rq-nav__title">登 录</text>
			<text class="rq-nav__slot">LV.12</text>
		</view>

		<view class="rq-body">
			<view class="rq-status">
				<view class="rq-face"><view class="pxi pxi-invader"></view></view>
				<view class="rq-status__meta">
					<text class="rq-status__name">HERO  LV.12  ·  冒险者</text>
					<view class="rq-bar rq-bar--hp">
						<view class="rq-bar__fill" style="width: 78%"></view>
						<text class="rq-bar__t">HP 78/100</text>
					</view>
					<view class="rq-bar rq-bar--mp">
						<view class="rq-bar__fill" style="width: 46%"></view>
						<text class="rq-bar__t">MP 46/100</text>
					</view>
				</view>
			</view>
			<view class="rq-card">
				<view class="rq-brand">
					<view class="rq-brand__logo"><view class="pxi pxi-invader"></view></view>
					<view class="rq-brand__meta">
						<text class="rq-brand__en">HERO ID</text>
						<text class="rq-brand__cn">冒险者档案</text>
					</view>
				</view>

				<text class="rq-title">登 录</text>
				<text class="rq-lead">载入存档，继续未完的冒险</text>

				<view class="rq-tabs">
					<view v-for="(item, i) in tabs" :key="i" class="rq-tab" :class="{ on: tab === i }" @click="tab = i">{{ item }}</view>
				</view>

				<view class="rq-form">
					<block v-if="tab === 0">
						<view class="rq-field">
							<view class="rq-field__ico"><view class="pxi pxi-user"></view></view>
							<input class="rq-field__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#a08e6d" maxlength="40" />
						</view>
						<view class="rq-field">
							<view class="rq-field__ico"><view class="pxi pxi-lock"></view></view>
							<input class="rq-field__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#a08e6d" maxlength="20" />
							<view class="rq-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
						<view class="rq-row">
							<text class="rq-row__link" @click="goForget">忘记密码？</text>
						</view>
					</block>
					<block v-else-if="tab === 1">
						<view class="rq-field">
							<view class="rq-field__ico"><view class="pxi pxi-phone"></view></view>
							<input class="rq-field__in rq-field__in--en" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#a08e6d" maxlength="11" />
						</view>
						<view class="rq-field">
							<view class="rq-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="rq-field__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#a08e6d" maxlength="6" />
							<view class="rq-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>
					<block v-else>
						<view class="rq-field">
							<view class="rq-field__ico"><view class="pxi pxi-mail"></view></view>
							<input class="rq-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#a08e6d" maxlength="40" />
						</view>
						<view class="rq-field">
							<view class="rq-field__ico"><view class="pxi pxi-shield"></view></view>
							<input class="rq-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#a08e6d" maxlength="6" />
							<view class="rq-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>

					<view class="rq-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>载入存档</text>
					</view>

					<view class="rq-foot">
						<text>还没有存档？</text>
						<text class="rq-foot__link" @click="goRegister">立即注册</text>
					</view>

					<view class="rq-agree" @click="agreed = !agreed">
						<view class="rq-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="rq-link">《用户协议》</text>与<text class="rq-link">《隐私政策》</text></text>
					</view>
				</view>
			</view>
			<view class="rq-quest">
				<view class="rq-quest__box">
					<text class="rq-quest__t">在告示板上签下你的名字，冒险即将开始</text>
					<view class="rq-quest__arrow"></view>
				</view>
				<view class="rq-chest">
					<view class="rq-chest__lid"></view>
					<view class="rq-chest__body"></view>
					<view class="rq-chest__lock"></view>
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
				uni.navigateTo({ url: '/pages/login96/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login96/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
