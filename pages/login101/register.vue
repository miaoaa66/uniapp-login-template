<template>
	<view class="nt-page">
		<view class="nt-boardpin nt-boardpin--lt"></view>
		<view class="nt-boardpin nt-boardpin--rt"></view>
		<view class="nt-boardpin nt-boardpin--lb"></view>
		<view class="nt-boardpin nt-boardpin--rb"></view>

		<view class="nt-tag" @click="goBack">
			<view class="nt-tag__pin"></view>
			<view class="pxi pxi-back"></view>
			<text class="nt-tag__t">任务板 · 注册</text>
		</view>

		<view class="nt-task nt-task--0">
			<view class="nt-pin"></view>
			<text class="nt-task__t">昵称</text>
			<view class="nt-line">
				<input class="nt-line__in" v-model="nickname" placeholder="2-12 字" placeholder-style="color:#8a6a4f" maxlength="12" />
			</view>
			<view v-if="okNick" class="nt-stamp"></view>
		</view>

		<view class="nt-task nt-task--1">
			<view class="nt-pin"></view>
			<text class="nt-task__t">邮箱</text>
			<view class="nt-line">
				<input class="nt-line__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#8a6a4f" maxlength="40" />
			</view>
			<view v-if="okMail" class="nt-stamp"></view>
		</view>

		<view class="nt-task nt-task--2">
			<view class="nt-pin"></view>
			<text class="nt-task__t">验证码</text>
			<view class="nt-line">
				<input class="nt-line__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#8a6a4f" maxlength="6" />
				<text class="nt-send" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '取验证码' }}</text>
			</view>
			<view v-if="okCode" class="nt-stamp"></view>
		</view>

		<view class="nt-task nt-task--3">
			<view class="nt-pin"></view>
			<text class="nt-task__t">密码</text>
			<view class="nt-line">
				<input class="nt-line__in" v-model="password" :password="!showPwd" placeholder="8-20 位" placeholder-style="color:#8a6a4f" maxlength="20" />
				<view class="nt-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
			</view>
			<view class="nt-ink">
				<text class="nt-ink__k">墨量</text>
				<view class="nt-ink__bar" :class="{ on: strength >= 1 }"></view>
				<view class="nt-ink__bar" :class="{ on: strength >= 2 }"></view>
				<view class="nt-ink__bar" :class="{ on: strength >= 3 }"></view>
				<text class="nt-ink__v">{{ strengthText }}</text>
			</view>
			<view v-if="okPwd" class="nt-stamp"></view>
		</view>

		<view class="nt-task nt-task--4">
			<view class="nt-pin"></view>
			<text class="nt-task__t">确认密码</text>
			<view class="nt-line">
				<input class="nt-line__in" v-model="password2" :password="!showPwd" placeholder="再写一遍" placeholder-style="color:#8a6a4f" maxlength="20" />
			</view>
			<view v-if="okPwd2" class="nt-stamp"></view>
		</view>

		<view class="nt-agree nt-agree--board" @click="agreed = !agreed">
			<view class="nt-check" :class="{ on: agreed }"></view>
			<text class="nt-agree__t">我已阅读并同意<text class="nt-agree__lnk">《用户协议》</text>与<text class="nt-agree__lnk">《隐私政策》</text></text>
		</view>

		<view class="nt-tapebtn nt-tapebtn--long" @click="submit">
			<text class="nt-tapebtn__t">全 部 完 成 · 提 交</text>
		</view>

		<view class="nt-scrap" @click="goLogin">
			<text class="nt-scrap__t">已有便签？返回登录 →</text>
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
			},
			// 字段有效判定（盖绿章用）
			okNick() {
				return !!this.nickname.trim()
			},
			okMail() {
				return this.isEmail(this.email)
			},
			okCode() {
				return !!this.mailCode
			},
			okPwd() {
				return this.password.length >= 8
			},
			okPwd2() {
				return this.password2.length > 0 && this.password === this.password2
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
