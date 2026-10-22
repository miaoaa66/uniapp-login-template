<template>
	<view class="tw-page">
		<!-- 登记表信纸 -->
		<view class="tw-paper">
			<view class="tw-clip" @click="goBack"><view class="tw-clip__inner"></view></view>

			<view class="tw-head">
				<view class="tw-head__line"></view>
				<view class="tw-head__mid">
					<text class="tw-head__en">REGISTRATION</text>
					<text class="tw-head__cn">登 记 表</text>
				</view>
				<view class="tw-head__line"></view>
			</view>

			<!-- 编号字段 -->
			<view class="tw-lines">
				<view class="tw-line">
					<text class="tw-line__label">1. 姓名</text>
					<view class="tw-line__blank">
						<input class="tw-line__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#b7b0a1" maxlength="12" />
					</view>
				</view>
				<view class="tw-line">
					<text class="tw-line__label">2. 邮箱</text>
					<view class="tw-line__blank">
						<input class="tw-line__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#b7b0a1" maxlength="40" />
					</view>
				</view>
				<view class="tw-line">
					<text class="tw-line__label">3. 印证</text>
					<view class="tw-line__blank">
						<input class="tw-line__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#b7b0a1" maxlength="6" />
						<text class="tw-word" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</text>
					</view>
				</view>
				<view class="tw-line">
					<text class="tw-line__label">4. 咒语</text>
					<view class="tw-line__blank">
						<input class="tw-line__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#b7b0a1" maxlength="20" />
						<view class="tw-line__eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>
				</view>
				<view class="tw-line">
					<text class="tw-line__label">5. 复述</text>
					<view class="tw-line__blank">
						<input class="tw-line__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#b7b0a1" maxlength="20" />
					</view>
				</view>
			</view>

			<!-- 油墨余量 = 密码强度 -->
			<view class="tw-ink">
				<text class="tw-ink__t">油墨余量</text>
				<view class="tw-ink__cells">
					<view class="tw-ink__cell" :class="{ on: strength >= 1 }"></view>
					<view class="tw-ink__cell" :class="{ on: strength >= 2 }"></view>
					<view class="tw-ink__cell" :class="{ on: strength >= 3 }"></view>
				</view>
				<text class="tw-ink__v">{{ strengthText }}</text>
			</view>

			<view class="tw-post">
				<view class="tw-agree" @click="agreed = !agreed">
					<view class="tw-check" :class="{ on: agreed }"></view>
					<text>我已阅读并同意<text class="tw-link">《用户协议》</text>与<text class="tw-link">《隐私政策》</text></text>
				</view>
				<text class="tw-post__zh">此致</text>
				<text class="tw-post__jing">敬礼！</text>
				<text class="tw-post__date">PIXEL TYPEWRITER CO. · 1983</text>
			</view>

			<view class="tw-keywrap">
				<view class="tw-ribbon" :class="{ go: ribbon }"></view>
				<view class="tw-key" @click="onSend"><text class="tw-key__t">寄 出</text></view>
			</view>

			<view class="tw-foot">
				<text class="tw-foot__link" @click="goLogin">已有档案？返回登录</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				ribbon: false,
				ribbonTimer: null,
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
				return ['余量不足', '余量不足', '尚可', '充足'][this.strength]
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
			if (this.ribbonTimer) clearTimeout(this.ribbonTimer)
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

			fireRibbon() {
				this.ribbon = false
				if (this.ribbonTimer) clearTimeout(this.ribbonTimer)
				setTimeout(() => { this.ribbon = true }, 30)
				this.ribbonTimer = setTimeout(() => { this.ribbon = false }, 800)
			},

			onSend() {
				this.fireRibbon()
				this.submit()
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
