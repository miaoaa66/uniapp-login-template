<template>
	<view class="ca-page">
		<!-- 周计划报名表 -->
		<view class="ca-sheet">
			<view class="ca-sheet__head">
				<view class="ca-sheet__back" @click="goBack"><view class="pxi pxi-back"></view></view>
				<view class="ca-sheet__meta">
					<text class="ca-sheet__cn">报名表</text>
					<text class="ca-sheet__en">WEEK PLAN</text>
				</view>
				<text class="ca-sheet__date">{{ calMonth }}月</text>
			</view>

			<!-- W1-W3 周格：每组填完自动打红勾 -->
			<view class="ca-weeks">
				<view class="ca-wk">
					<view class="ca-wk__meta"><text class="ca-wk__no">W1</text><text class="ca-wk__t">名字</text></view>
					<view class="ca-wk__check" :class="{ on: g1Ok }"></view>
				</view>
				<view class="ca-wk">
					<view class="ca-wk__meta"><text class="ca-wk__no">W2</text><text class="ca-wk__t">联络</text></view>
					<view class="ca-wk__check" :class="{ on: g2Ok }"></view>
				</view>
				<view class="ca-wk ca-wk--last">
					<view class="ca-wk__meta"><text class="ca-wk__no">W3</text><text class="ca-wk__t">密码</text></view>
					<view class="ca-wk__check" :class="{ on: g3Ok }"></view>
				</view>
			</view>

			<view class="ca-form">
				<text class="ca-form__cap">第一周 · 填名字</text>
				<view class="ca-line">
					<text class="ca-line__tag">昵称</text>
					<input class="ca-line__in" v-model="nickname" placeholder="2-12 字" placeholder-style="color:#9a9a9a" maxlength="12" />
				</view>

				<text class="ca-form__cap">第二周 · 留联络</text>
				<view class="ca-line">
					<text class="ca-line__tag">邮箱</text>
					<input class="ca-line__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#9a9a9a" maxlength="40" />
				</view>
				<view class="ca-line">
					<text class="ca-line__tag">验证</text>
					<input class="ca-line__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#9a9a9a" maxlength="6" />
					<view class="ca-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
				</view>

				<text class="ca-form__cap">第三周 · 设密码</text>
				<view class="ca-line">
					<text class="ca-line__tag">密码</text>
					<input class="ca-line__in" v-model="password" :password="!showPwd" placeholder="8-20 位" placeholder-style="color:#9a9a9a" maxlength="20" />
					<view class="ca-line__eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
				</view>
				<view class="ca-line">
					<text class="ca-line__tag">确认</text>
					<input class="ca-line__in" v-model="password2" :password="!showPwd" placeholder="再输一遍" placeholder-style="color:#9a9a9a" maxlength="20" />
				</view>
				<view class="ca-ink">
					<text class="ca-ink__t">墨量</text>
					<view class="ca-ink__bar" :class="{ on: strength >= 1 }"></view>
					<view class="ca-ink__bar" :class="{ on: strength >= 2 }"></view>
					<view class="ca-ink__bar" :class="{ on: strength >= 3 }"></view>
					<text class="ca-ink__v">{{ strengthText }}</text>
				</view>

				<view class="ca-btn" @click="submit"><text>完 成 报 名</text></view>

				<view class="ca-foot">
					<text>已有账户？</text>
					<text class="ca-foot__link" @click="goLogin">返回登录</text>
				</view>

				<view class="ca-agree" @click="agreed = !agreed">
					<view class="ca-check" :class="{ on: agreed }"></view>
					<text>我已阅读并同意<text class="ca-link">《用户协议》</text>与<text class="ca-link">《隐私政策》</text></text>
				</view>
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
				calMonth: new Date().getMonth() + 1
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
			},
			// 周格红勾：每组输入有效即点亮
			g1Ok() {
				return !!this.nickname
			},
			g2Ok() {
				return this.isEmail(this.email) && !!this.mailCode
			},
			g3Ok() {
				return this.password.length >= 8 && this.password === this.password2
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
