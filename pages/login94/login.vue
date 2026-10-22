<template>
	<view class="cr-scr">
		<!-- 顶部状态栏 -->
		<view class="cr-bar">
			<view class="cr-bar__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="cr-bar__t">PIXEL-OS // AUTH NODE</text>
			<text class="cr-bar__r">CREDIT 01</text>
		</view>

		<!-- 扫描线 overlay -->
		<view class="cr-scan"></view>

		<!-- 终端主体 -->
		<view class="cr-term">
			<!-- boot 日志（固定高度，逐行打印） -->
			<view class="cr-boot">
				<text class="cr-boot__l cr-boot__l--1">PIXEL-OS v2.6 .......... OK</text>
				<text class="cr-boot__l cr-boot__l--2">LOADING AUTH MODULE .... OK</text>
				<text class="cr-boot__l cr-boot__l--3">LINK://202.6.14.3 ...... OK</text>
				<text class="cr-boot__l cr-boot__l--4">READY <text class="cr-caret">▉</text></text>
			</view>

			<text class="cr-h cr-h--d1">AUTH://SELECT MODE</text>

			<!-- 模式命令旗标 -->
			<view class="cr-flags cr-flags--d2">
				<view class="cr-flag" :class="{ on: tab === 0 }" @click="tab = 0">[PWD]</view>
				<view class="cr-flag" :class="{ on: tab === 1 }" @click="tab = 1">[SMS]</view>
				<view class="cr-flag" :class="{ on: tab === 2 }" @click="tab = 2">[MAIL]</view>
			</view>

			<!-- 命令行输入区 -->
			<view class="cr-lines cr-lines--d3">
				<block v-if="tab === 0">
					<view class="cr-line">
						<text class="cr-line__p">&gt;</text>
						<input class="cr-line__in" v-model="account" placeholder="USER / ACCOUNT" placeholder-style="color:#2b4a44" maxlength="40" />
					</view>
					<view class="cr-line">
						<text class="cr-line__p">&gt;</text>
						<input class="cr-line__in" v-model="password" :password="!showPwd" placeholder="PASSPHRASE" placeholder-style="color:#2b4a44" maxlength="20" />
						<view class="cr-line__ext" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>
					<view class="cr-line cr-line--lnk">
						<text class="cr-line__p">&gt;</text>
						<text class="cr-lnk" @click="goForget">FORGOT? /RESET</text>
					</view>
				</block>
				<block v-else-if="tab === 1">
					<view class="cr-line">
						<text class="cr-line__p">&gt;</text>
						<input class="cr-line__in cr-line__in--num" v-model="phone" type="number" placeholder="PHONE +86" placeholder-style="color:#2b4a44" maxlength="11" />
					</view>
					<view class="cr-line">
						<text class="cr-line__p">&gt;</text>
						<input class="cr-line__in cr-line__in--num" v-model="smsCode" type="number" placeholder="SMS CODE" placeholder-style="color:#2b4a44" maxlength="6" />
						<text class="cr-send" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '[SEND]' }}</text>
					</view>
				</block>
				<block v-else>
					<view class="cr-line">
						<text class="cr-line__p">&gt;</text>
						<input class="cr-line__in" v-model="email" placeholder="MAIL ADDR" placeholder-style="color:#2b4a44" maxlength="40" />
					</view>
					<view class="cr-line">
						<text class="cr-line__p">&gt;</text>
						<input class="cr-line__in cr-line__in--num" v-model="mailCode" type="number" placeholder="MAIL CODE" placeholder-style="color:#2b4a44" maxlength="6" />
						<text class="cr-send" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '[SEND]' }}</text>
					</view>
				</block>

				<view class="cr-line cr-line--lnk">
					<text class="cr-line__p">&gt;</text>
					<text class="cr-lnk" @click="goRegister">NEW USER? /REGISTER</text>
				</view>

				<view class="cr-agree cr-agree--d4" @click="agreed = !agreed">
					<text class="cr-agree__box">[{{ agreed ? 'X' : ' ' }}]</text>
					<text class="cr-agree__t">我已阅读并同意《用户协议》与《隐私政策》</text>
				</view>
			</view>

			<!-- EXECUTE -->
			<view class="cr-exec" :class="{ glitch: glitch }" @click="exec">
				<text>[ EXECUTE ]</text>
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
				glitch: false
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

			// 按下 EXECUTE：先 glitch 抖动，再走校验
			exec() {
				if (this.glitch) return
				this.glitch = true
				setTimeout(() => {
					this.glitch = false
					this.submit()
				}, 220)
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
				uni.navigateTo({ url: '/pages/login94/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login94/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
