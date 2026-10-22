<template>
	<view class="sp-page">
		<view class="sp-dome">
			<view class="sp-exit" @click="goBack">
				<text class="sp-exit__t">EXIT ▸</text>
			</view>
			<view class="sp-tick sp-tick--lt"></view>
			<view class="sp-tick sp-tick--rt"></view>
			<view class="sp-tick sp-tick--lb"></view>
			<view class="sp-tick sp-tick--rb"></view>
			<view class="sp-dome__hud sp-dome__hud--lt"><text class="sp-dome__r">LAT 31.02N</text></view>
			<view class="sp-dome__hud sp-dome__hud--rt"><text class="sp-dome__r">LON 121.45E</text></view>
			<view class="sp-dome__hud sp-dome__hud--lb"><text class="sp-dome__r">SIG 98.2%</text></view>
			<view class="sp-dome__hud sp-dome__hud--rb"><text class="sp-dome__r">TGT 03</text></view>
			<view class="sp-radar">
				<view class="sp-radar__ring sp-radar__ring--out"></view>
				<view class="sp-radar__ring sp-radar__ring--mid"></view>
				<view class="sp-radar__ring sp-radar__ring--in"></view>
				<view class="sp-radar__cross"></view>
				<view class="sp-radar__sweep"></view>
				<view class="sp-radar__blip sp-radar__blip--a"></view>
				<view class="sp-radar__blip sp-radar__blip--b"></view>
				<view class="sp-radar__blip sp-radar__blip--c"></view>
			</view>
		</view>

		<view class="sp-console">
			<view class="sp-chrow">
				<view v-for="(item, i) in tabs" :key="i" class="sp-ch" :class="{ on: tab === i }" @click="tab = i">
					<view class="sp-ch__led"></view>
					<text class="sp-ch__t">CH-0{{ i + 1 }} {{ item }}</text>
				</view>
			</view>

			<view class="sp-form">
				<block v-if="tab === 0">
					<view class="sp-line" :class="{ lock: focusKey === 'account' }">
						<text class="sp-line__k">ACCNT ▸</text>
						<input class="sp-line__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#3d5a52" maxlength="40" @focus="focusKey = 'account'" @blur="focusKey = ''" />
						<view class="sp-line__c sp-line__c--tr"></view>
						<view class="sp-line__c sp-line__c--bl"></view>
					</view>
					<view class="sp-line" :class="{ lock: focusKey === 'password' }">
						<text class="sp-line__k">KEYWD ▸</text>
						<input class="sp-line__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#3d5a52" maxlength="20" @focus="focusKey = 'password'" @blur="focusKey = ''" />
						<view class="sp-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						<view class="sp-line__c sp-line__c--tr"></view>
						<view class="sp-line__c sp-line__c--bl"></view>
					</view>
					<view class="sp-mini"><text class="sp-mini__t" @click="goForget">忘记密码？SEND SOS ▸</text></view>
				</block>
				<block v-else-if="tab === 1">
					<view class="sp-line" :class="{ lock: focusKey === 'phone' }">
						<text class="sp-line__k">PHONE ▸</text>
						<input class="sp-line__in" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#3d5a52" maxlength="11" @focus="focusKey = 'phone'" @blur="focusKey = ''" />
						<view class="sp-line__c sp-line__c--tr"></view>
						<view class="sp-line__c sp-line__c--bl"></view>
					</view>
					<view class="sp-line" :class="{ lock: focusKey === 'smsCode' }">
						<text class="sp-line__k">CODE ▸</text>
						<input class="sp-line__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#3d5a52" maxlength="6" @focus="focusKey = 'smsCode'" @blur="focusKey = ''" />
						<view class="sp-send" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')"><text class="sp-send__t">{{ smsLeft > 0 ? smsLeft + 's' : 'SEND ▸' }}</text></view>
						<view class="sp-line__c sp-line__c--tr"></view>
						<view class="sp-line__c sp-line__c--bl"></view>
					</view>
				</block>
				<block v-else>
					<view class="sp-line" :class="{ lock: focusKey === 'email' }">
						<text class="sp-line__k">MAILT ▸</text>
						<input class="sp-line__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#3d5a52" maxlength="40" @focus="focusKey = 'email'" @blur="focusKey = ''" />
						<view class="sp-line__c sp-line__c--tr"></view>
						<view class="sp-line__c sp-line__c--bl"></view>
					</view>
					<view class="sp-line" :class="{ lock: focusKey === 'mailCode' }">
						<text class="sp-line__k">CODE ▸</text>
						<input class="sp-line__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#3d5a52" maxlength="6" @focus="focusKey = 'mailCode'" @blur="focusKey = ''" />
						<view class="sp-send" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')"><text class="sp-send__t">{{ mailLeft > 0 ? mailLeft + 's' : 'SEND ▸' }}</text></view>
						<view class="sp-line__c sp-line__c--tr"></view>
						<view class="sp-line__c sp-line__c--bl"></view>
					</view>
				</block>

				<view class="sp-engage" :class="{ charging: charging }" @click="submit">
					<view class="sp-engage__charge"></view>
					<text class="sp-engage__t">ENGAGE ▸ 接入主控</text>
				</view>

				<view class="sp-proto" @click="agreed = !agreed">
					<text class="sp-proto__k">PROTOCOL ACCEPT</text>
					<view class="sp-proto__box" :class="{ on: agreed }"></view>
					<text class="sp-proto__t">我已阅读并同意<text class="sp-proto__lnk">《用户协议》</text>与<text class="sp-proto__lnk">《隐私政策》</text></text>
				</view>

				<view class="sp-foot">
					<text class="sp-foot__t">尚无船员编号？</text>
					<text class="sp-foot__link" @click="goRegister">ENLIST ▸ 立即注册</text>
				</view>

				<view v-if="okFlash" class="sp-flash"><text class="sp-flash__t">LINK ESTABLISHED · 已接入</text></view>
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
				focusKey: '',
				charging: false,
				okFlash: false
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

			// 按下后充电条 0.6s 充满再执行校验
			submit() {
				if (this.charging) return
				this.charging = true
				setTimeout(() => {
					this.charging = false
					this.doSubmit()
				}, 600)
			},

			doSubmit() {
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
				this.okFlash = true
				setTimeout(() => { this.okFlash = false }, 2200)
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
