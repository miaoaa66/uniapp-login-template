<template>
	<view class="cr-scr">
		<!-- 顶部状态栏 -->
		<view class="cr-bar">
			<view class="cr-bar__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="cr-bar__t">PIXEL-OS // NEW USER REGISTRY</text>
			<text class="cr-bar__r">UID ---</text>
		</view>

		<view class="cr-scan"></view>

		<view class="cr-term">
			<!-- boot 日志 -->
			<view class="cr-boot">
				<text class="cr-boot__l cr-boot__l--1">PIXEL-OS v2.6 .......... OK</text>
				<text class="cr-boot__l cr-boot__l--2">REGISTRY MODULE ........ OK</text>
				<text class="cr-boot__l cr-boot__l--3">READY <text class="cr-caret">▉</text></text>
			</view>

			<text class="cr-h cr-h--d1">NEW USER REGISTRY</text>

			<!-- 命令行表单 -->
			<view class="cr-lines cr-lines--d2">
				<view class="cr-line">
					<text class="cr-line__k">NICK&gt;</text>
					<input class="cr-line__in" v-model="nickname" placeholder="2-12 CHARS" placeholder-style="color:#2b4a44" maxlength="12" />
				</view>

				<view class="cr-line">
					<text class="cr-line__k">MAIL&gt;</text>
					<input class="cr-line__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#2b4a44" maxlength="40" />
				</view>

				<view class="cr-line">
					<text class="cr-line__k">CODE&gt;</text>
					<input class="cr-line__in cr-line__in--num" v-model="mailCode" type="number" placeholder="MAIL CODE" placeholder-style="color:#2b4a44" maxlength="6" />
					<text class="cr-send" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '[SEND]' }}</text>
				</view>

				<view class="cr-line">
					<text class="cr-line__k">PASS&gt;</text>
					<input class="cr-line__in" v-model="password" :password="!showPwd" placeholder="8-20 CHARS" placeholder-style="color:#2b4a44" maxlength="20" />
					<view class="cr-line__ext" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
				</view>

				<view class="cr-line">
					<text class="cr-line__k">CONF&gt;</text>
					<input class="cr-line__in" v-model="password2" :password="!showPwd" placeholder="REPEAT" placeholder-style="color:#2b4a44" maxlength="20" />
				</view>

				<!-- SIGNAL 信号条 -->
				<view class="cr-sig">
					<text class="cr-sig__t">SIGNAL [{{ sigLitText }}]</text>
					<view class="cr-sig__bars">
						<view v-for="n in 5" :key="n" class="cr-sig__b" :class="{ on: sigLit >= n }"></view>
					</view>
				</view>

				<view class="cr-agree" @click="agreed = !agreed">
					<text class="cr-agree__box">[{{ agreed ? 'X' : ' ' }}]</text>
					<text class="cr-agree__t">我已阅读并同意《用户协议》与《隐私政策》</text>
				</view>

				<view class="cr-line cr-line--lnk">
					<text class="cr-line__p">&gt;</text>
					<text class="cr-lnk" @click="goLogin">HAVE ID? /LOGIN</text>
				</view>
			</view>

			<view class="cr-exec" :class="{ glitch: glitch }" @click="exec">
				<text>[ REGISTER ]</text>
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
				glitch: false
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
			// SIGNAL 阶梯：>=1 亮 2 格 / >=2 亮 3 格 / >=3 亮 5 格
			sigLit() {
				if (this.strength >= 3) return 5
				if (this.strength >= 2) return 3
				if (this.strength >= 1) return 2
				return 0
			},
			sigLitText() {
				return ['----', 'WEAK', 'MID', 'FULL'][this.strength]
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

			exec() {
				if (this.glitch) return
				this.glitch = true
				setTimeout(() => {
					this.glitch = false
					this.submit()
				}, 220)
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
