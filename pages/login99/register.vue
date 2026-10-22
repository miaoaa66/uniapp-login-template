<template>
	<view class="tk-page">
		<view class="tk-top">
			<view class="tk-back" @click="goBack"><view class="pxi pxi-back"></view></view>
			<text class="tk-top__t">联程票 · 换乘登记</text>
		</view>

		<!-- 三张连票 -->
		<view class="tk-chain">
			<!-- 第一联：昵称 -->
			<view class="tk-joint">
				<view class="tk-joint__stub">
					<text class="tk-joint__no">01</text>
					<block v-if="punched.a">
						<view class="tk-joint__hole"></view>
						<text class="tk-joint__ok">✓</text>
					</block>
				</view>
				<view class="tk-joint__main">
					<text class="tk-joint__t">乘客姓名</text>
					<view class="tk-field">
						<input class="tk-field__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#93a3b8" maxlength="12" />
					</view>
					<view class="tk-punch" :class="{ done: punched.a }" @click="punchCheck(1)">{{ punched.a ? '已打孔' : '打孔' }}</view>
				</view>
			</view>
			<view class="tk-linkline"></view>

			<!-- 第二联：邮箱 + 验证码 -->
			<view class="tk-joint">
				<view class="tk-joint__stub">
					<text class="tk-joint__no">02</text>
					<block v-if="punched.b">
						<view class="tk-joint__hole"></view>
						<text class="tk-joint__ok">✓</text>
					</block>
				</view>
				<view class="tk-joint__main">
					<text class="tk-joint__t">换乘站</text>
					<view class="tk-field">
						<input class="tk-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#93a3b8" maxlength="40" />
					</view>
					<view class="tk-field">
						<input class="tk-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#93a3b8" maxlength="6" />
						<text class="tk-get" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</text>
					</view>
					<view class="tk-punch" :class="{ done: punched.b }" @click="punchCheck(2)">{{ punched.b ? '已打孔' : '打孔' }}</view>
				</view>
			</view>
			<view class="tk-linkline"></view>

			<!-- 第三联：密码 + 确认 -->
			<view class="tk-joint">
				<view class="tk-joint__stub">
					<text class="tk-joint__no">03</text>
					<block v-if="punched.c">
						<view class="tk-joint__hole"></view>
						<text class="tk-joint__ok">✓</text>
					</block>
				</view>
				<view class="tk-joint__main">
					<text class="tk-joint__t">终点站</text>
					<view class="tk-field">
						<input class="tk-field__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#93a3b8" maxlength="20" />
						<view class="tk-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>
					<view class="tk-field">
						<input class="tk-field__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#93a3b8" maxlength="20" />
					</view>
					<view class="tk-power">
						<text class="tk-power__t">强度</text>
						<view class="tk-power__bars">
							<view class="tk-power__bar" :class="{ on: strength >= 1 }"></view>
							<view class="tk-power__bar" :class="{ on: strength >= 2 }"></view>
							<view class="tk-power__bar" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="tk-power__v">{{ strengthText }}</text>
					</view>
					<view class="tk-punch" :class="{ done: punched.c }" @click="punchCheck(3)">{{ punched.c ? '已打孔' : '打孔' }}</view>
				</view>
			</view>

			<!-- 协议 -->
			<view class="tk-agree" @click="agreed = !agreed">
				<view class="tk-dot" :class="{ on: agreed }"></view>
				<text>我已阅读并同意<text class="tk-agree__link">《用户协议》</text>与<text class="tk-agree__link">《隐私政策》</text></text>
			</view>

			<view class="tk-btn" @click="submit"><text class="tk-btn__t">验 票</text></view>

			<view class="tk-foot">
				<text class="tk-foot__link" @click="goLogin">已有车票？返回登录</text>
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
				punched: { a: false, b: false, c: false },
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

			// 每张票完成校验后打孔
			punchCheck(n) {
				if (n === 1) {
					if (!this.nickname) return this.toast('请输入昵称')
					this.punched.a = true
				} else if (n === 2) {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (!this.mailCode) return this.toast('请输入邮箱验证码')
					this.punched.b = true
				} else {
					if (this.password.length < 8) return this.toast('密码至少 8 位')
					if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
					this.punched.c = true
				}
			},

			// 统一提交：完整校验顺序（含协议）
			submit() {
				if (!this.nickname) return this.toast('请输入昵称')
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (!this.mailCode) return this.toast('请输入邮箱验证码')
				if (this.password.length < 8) return this.toast('密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端注册接口
				this.toast('注册成功')
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
