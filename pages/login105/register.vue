<template>
	<view class="vd-page">
		<!-- 机身：顶盖灯箱 + 补货面板 -->
		<view class="vd-machine">
			<view class="vd-cap">
				<view class="vd-cap__back" @click="goBack"><view class="pxi pxi-back"></view></view>
				<view class="vd-sign">
					<view class="vd-sign__lamp"></view>
					<text class="vd-sign__t">补货服务 · REFILL 24H</text>
					<view class="vd-sign__lamp vd-sign__lamp--r"></view>
				</view>
			</view>

			<view class="vd-panel">
				<text class="vd-panel__title">补货员登记</text>
				<text class="vd-panel__sub">登记后你的专属货道即刻上架</text>

				<!-- 货位号风格字段 -->
				<view class="vd-line">
					<text class="vd-line__no">SLOT-01</text>
					<input class="vd-line__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#8b97a8" maxlength="12" />
				</view>
				<view class="vd-line">
					<text class="vd-line__no">SLOT-02</text>
					<input class="vd-line__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#8b97a8" maxlength="40" />
				</view>
				<view class="vd-line">
					<text class="vd-line__no">SLOT-03</text>
					<input class="vd-line__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#8b97a8" maxlength="6" />
					<view class="vd-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
				</view>
				<view class="vd-line">
					<text class="vd-line__no">SLOT-04</text>
					<input class="vd-line__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#8b97a8" maxlength="20" />
					<view class="vd-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
				</view>
				<view class="vd-line">
					<text class="vd-line__no">SLOT-05</text>
					<input class="vd-line__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#8b97a8" maxlength="20" />
				</view>

				<view class="vd-batt">
					<text class="vd-batt__t">电量</text>
					<view class="vd-batt__bar" :class="{ on: strength >= 1 }"></view>
					<view class="vd-batt__bar" :class="{ on: strength >= 2 }"></view>
					<view class="vd-batt__bar" :class="{ on: strength >= 3 }"></view>
					<text class="vd-batt__v">{{ strengthText }}</text>
				</view>

				<view class="vd-btn" @click="submit"><text>完 成 上 架</text></view>

				<view class="vd-agree" @click="agreed = !agreed">
					<view class="vd-check" :class="{ on: agreed }"></view>
					<text>我已阅读并同意<text class="vd-link">《用户协议》</text>与<text class="vd-link">《隐私政策》</text></text>
				</view>

				<view class="vd-foot">
					<text>已经是会员？</text>
					<text class="vd-foot__link" @click="goLogin">返回登录</text>
				</view>
			</view>

			<view class="vd-hatch"><view class="vd-hatch__door"><text>PUSH</text></view></view>
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
				timer: null
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
				return ['──', '弱', '中', '强'][this.strength]
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

			submit() {
				if (!this.nickname) return this.toast('请输入昵称')
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (!this.mailCode) return this.toast('请输入邮箱验证码')
				if (this.password.length < 8) return this.toast('密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端注册接口
				this.toast('注册成功，商品已上架')
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
