<template>
	<view class="rq-page">
		<view class="rq-nav">
			<view class="rq-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="rq-nav__title">注 册</text>
			<text class="rq-nav__slot">NEW</text>
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
					<view class="rq-brand__logo"><view class="pxi pxi-user"></view></view>
					<view class="rq-brand__meta">
						<text class="rq-brand__en">NEW HERO</text>
						<text class="rq-brand__cn">创建冒险者</text>
					</view>
				</view>

				<text class="rq-title">创 建 角 色</text>
				<text class="rq-lead">取一个名字，从新手村出发</text>

				<view class="rq-form">
					<view class="rq-field">
						<view class="rq-field__ico"><view class="pxi pxi-user"></view></view>
						<input class="rq-field__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#a08e6d" maxlength="12" />
					</view>

					<view class="rq-field">
						<view class="rq-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="rq-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#a08e6d" maxlength="40" />
					</view>

					<view class="rq-field">
						<view class="rq-field__ico"><view class="pxi pxi-shield"></view></view>
						<input class="rq-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#a08e6d" maxlength="6" />
						<view class="rq-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="rq-field">
						<view class="rq-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="rq-field__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#a08e6d" maxlength="20" />
						<view class="rq-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="rq-field">
						<view class="rq-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="rq-field__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#a08e6d" maxlength="20" />
					</view>

					<view class="rq-power">
						<text class="rq-power__t">密码强度</text>
						<view class="rq-power__bars">
							<view class="rq-power__bar" :class="{ on: strength >= 1 }"></view>
							<view class="rq-power__bar" :class="{ on: strength >= 2 }"></view>
							<view class="rq-power__bar" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="rq-power__v">{{ strengthText }}</text>
					</view>

					<view class="rq-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>开始冒险</text>
					</view>

					<view class="rq-foot">
						<text>已有存档？</text>
						<text class="rq-foot__link" @click="goLogin">返回登录</text>
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
