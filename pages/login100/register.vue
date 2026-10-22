<template>
	<view class="sp-page">
		<view class="sp-dome sp-dome--slim">
			<view class="sp-exit" @click="goBack">
				<text class="sp-exit__t">EXIT ▸</text>
			</view>
			<view class="sp-tick sp-tick--lt"></view>
			<view class="sp-tick sp-tick--rt"></view>
			<view class="sp-tick sp-tick--lb"></view>
			<view class="sp-tick sp-tick--rb"></view>
			<view class="sp-radar">
				<view class="sp-radar__ring sp-radar__ring--out"></view>
				<view class="sp-radar__ring sp-radar__ring--mid"></view>
				<view class="sp-radar__ring sp-radar__ring--in"></view>
				<view class="sp-radar__cross"></view>
				<view class="sp-radar__sweep"></view>
				<view class="sp-radar__blip sp-radar__blip--a"></view>
				<view class="sp-radar__blip sp-radar__blip--b"></view>
			</view>
			<text class="sp-dome__cap">ENLIST PROTOCOL · 登记序列</text>
		</view>

		<view class="sp-console">
			<view class="sp-form">
				<view class="sp-line" :class="{ lock: focusKey === 'nickname' }">
					<text class="sp-line__k">CREW NAME ▸</text>
					<input class="sp-line__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#3d5a52" maxlength="12" @focus="focusKey = 'nickname'" @blur="focusKey = ''" />
					<view class="sp-line__c sp-line__c--tr"></view>
					<view class="sp-line__c sp-line__c--bl"></view>
				</view>

				<view class="sp-line" :class="{ lock: focusKey === 'email' }">
					<text class="sp-line__k">COMMS ▸</text>
					<input class="sp-line__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#3d5a52" maxlength="40" @focus="focusKey = 'email'" @blur="focusKey = ''" />
					<view class="sp-send" :class="{ off: mailLeft > 0 }" @click="sendCode"><text class="sp-send__t">{{ mailLeft > 0 ? mailLeft + 's' : 'SEND ▸' }}</text></view>
					<view class="sp-line__c sp-line__c--tr"></view>
					<view class="sp-line__c sp-line__c--bl"></view>
				</view>

				<view class="sp-line" :class="{ lock: focusKey === 'mailCode' }">
					<text class="sp-line__k">VERIF ▸</text>
					<input class="sp-line__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#3d5a52" maxlength="6" @focus="focusKey = 'mailCode'" @blur="focusKey = ''" />
					<view class="sp-line__c sp-line__c--tr"></view>
					<view class="sp-line__c sp-line__c--bl"></view>
				</view>

				<view class="sp-line" :class="{ lock: focusKey === 'password' }">
					<text class="sp-line__k">ACCESS KEY ▸</text>
					<input class="sp-line__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#3d5a52" maxlength="20" @focus="focusKey = 'password'" @blur="focusKey = ''" />
					<view class="sp-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					<view class="sp-line__c sp-line__c--tr"></view>
					<view class="sp-line__c sp-line__c--bl"></view>
				</view>

				<view class="sp-line" :class="{ lock: focusKey === 'password2' }">
					<text class="sp-line__k">CONFIRM ▸</text>
					<input class="sp-line__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#3d5a52" maxlength="20" @focus="focusKey = 'password2'" @blur="focusKey = ''" />
					<view class="sp-line__c sp-line__c--tr"></view>
					<view class="sp-line__c sp-line__c--bl"></view>
				</view>

				<view class="sp-energy">
					<text class="sp-energy__k">ENERGY</text>
					<view class="sp-energy__bar" :class="{ on: strength >= 1 }"></view>
					<view class="sp-energy__bar" :class="{ on: strength >= 2 }"></view>
					<view class="sp-energy__bar" :class="{ on: strength >= 3 }"></view>
					<text class="sp-energy__v">{{ strengthText }}</text>
				</view>

				<view class="sp-engage" :class="{ charging: charging }" @click="submit">
					<view class="sp-engage__charge"></view>
					<text class="sp-engage__t">ENLIST ▸ 提交登记</text>
				</view>

				<view class="sp-proto" @click="agreed = !agreed">
					<text class="sp-proto__k">PROTOCOL ACCEPT</text>
					<view class="sp-proto__box" :class="{ on: agreed }"></view>
					<text class="sp-proto__t">我已阅读并同意<text class="sp-proto__lnk">《用户协议》</text>与<text class="sp-proto__lnk">《隐私政策》</text></text>
				</view>

				<view class="sp-foot">
					<text class="sp-foot__t">已有编号？</text>
					<text class="sp-foot__link" @click="goLogin">RELINK ▸ 返回登录</text>
				</view>

				<view v-if="okFlash" class="sp-flash"><text class="sp-flash__t">REGISTRY CONFIRMED · 登记确认</text></view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				agreed: false,
				showPwd: false,
				nickname: '',
				email: '',
				mailCode: '',
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null,
				focusKey: '',
				charging: false,
				okFlash: false
			}
		},
		computed: {
			strength() {
				const v = this.password
				if (!v) return 0
				let s = 0
				if (v.length >= 8) s++
				if (/[a-zA-Z]/.test(v) && /\d/.test(v)) s++
				if (/[^a-zA-Z0-9]/.test(v)) s++
				return s
			},
			strengthText() {
				return ['──', 'LOW', 'MID', 'FULL'][this.strength]
			}
		},
		onLoad(options) {
			if (options && options.demo) {
				this.agreed = true
				this.nickname = 'PLAYER-1'
				this.email = 'player@pixel.id'
				this.mailCode = '1983'
				this.password = '1up-1983'
				this.password2 = '1up-1983'
			}
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			toast(title) {
				uni.showToast({ title, icon: 'none' })
			},

			isEmail(v) {
				return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v)
			},

			sendCode() {
				if (this.mailLeft > 0) return
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				this.mailLeft = 60
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发送')
				if (this.timer) return
				this.timer = setInterval(() => {
					if (this.mailLeft > 0) this.mailLeft--
					else this.clearTimer()
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
				if (!this.nickname) return this.toast('请输入昵称')
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (!this.mailCode) return this.toast('请输入邮箱验证码')
				if (this.password.length < 8) return this.toast('密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端注册接口
				this.toast('注册成功')
				this.okFlash = true
				setTimeout(() => { this.okFlash = false }, 2200)
			},

			goBack() {
				uni.navigateBack()
			},

			goLogin() {
				uni.navigateBack()
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
