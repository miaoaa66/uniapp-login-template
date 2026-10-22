<template>
	<view class="ac-page">
		<!-- 顶部 marquee 灯箱 -->
		<view class="ac-marquee">
			<view class="ac-marquee__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<view class="ac-bulbs">
				<view v-for="n in 4" :key="'l'+n" class="ac-bulb" :class="n % 2 ? 'ac-bulb--a' : 'ac-bulb--b'"></view>
			</view>
			<text class="ac-marquee__t">NEW PLAYER</text>
			<view class="ac-bulbs">
				<view v-for="n in 4" :key="'r'+n" class="ac-bulb" :class="n % 2 ? 'ac-bulb--b' : 'ac-bulb--a'"></view>
			</view>
		</view>

		<!-- 投币口（注册免投币，仅装饰） -->
		<view class="ac-slotrow">
			<text class="ac-slotrow__t">FREE PLAY</text>
			<view class="ac-slot"></view>
		</view>

		<!-- 屏幕 -->
		<view class="ac-screen">
			<!-- select：唯一选项 1P REGISTER -->
			<block v-if="phase === 'select'">
				<text class="ac-scr__h">SELECT MODE</text>
				<view class="ac-mode" @click="pickMode">
					<view class="ac-mode__cur on"></view>
					<text class="ac-mode__t">1P REGISTER</text>
				</view>
				<text class="ac-hint">CREDIT 1 · FREE PLAY</text>
			</block>

			<!-- form：注册表单 -->
			<block v-else>
				<view class="ac-f">
					<text class="ac-f__k">ENTER YOUR TAG</text>
					<input class="ac-f__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#5a5470" maxlength="12" />
				</view>

				<view class="ac-f">
					<text class="ac-f__k">MAIL ADDR</text>
					<input class="ac-f__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#5a5470" maxlength="40" />
				</view>

				<view class="ac-f">
					<text class="ac-f__k">MAIL CODE</text>
					<input class="ac-f__in ac-f__in--num" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#5a5470" maxlength="6" />
					<text class="ac-f__send" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : 'SEND' }}</text>
				</view>

				<view class="ac-f">
					<text class="ac-f__k">PASSWORD</text>
					<input class="ac-f__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#5a5470" maxlength="20" />
					<view class="ac-f__eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
				</view>

				<view class="ac-f">
					<text class="ac-f__k">REPEAT</text>
					<input class="ac-f__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#5a5470" maxlength="20" />
				</view>

				<!-- LIFE 三格（绿） -->
				<view class="ac-life">
					<text class="ac-life__t">LIFE</text>
					<view class="ac-life__bars">
						<view class="ac-life__b" :class="{ on: strength >= 1 }"></view>
						<view class="ac-life__b" :class="{ on: strength >= 2 }"></view>
						<view class="ac-life__b" :class="{ on: strength >= 3 }"></view>
					</view>
					<text class="ac-life__v">{{ strengthText }}</text>
				</view>

				<view class="ac-f ac-f--lnk">
					<text class="ac-f__lnk" @click="goLogin">已经有档案了？返回登录</text>
				</view>

				<view class="ac-agree" @click="agreed = !agreed">
					<view class="ac-agree__box" :class="{ on: agreed }"></view>
					<text class="ac-agree__t">我已阅读并同意《用户协议》与《隐私政策》</text>
				</view>

				<text class="ac-push">PUSH START</text>
			</block>

			<text class="ac-credit-mini">CREDIT {{ credit }}</text>
		</view>

		<!-- 控制面板 -->
		<view class="ac-panel">
			<view class="ac-stick">
				<view class="ac-stick__ball"></view>
				<view class="ac-stick__rod"></view>
				<view class="ac-stick__base"></view>
			</view>
			<view class="ac-btns">
				<view class="ac-ab ac-ab--b" @click="pressB"><text>B</text></view>
				<view class="ac-ab ac-ab--a" @click="pressA"><text>A</text></view>
			</view>
		</view>

		<view class="ac-hud">
			<text class="ac-hud__l">1UP 004500</text>
			<text class="ac-hud__l">HIGH 012800</text>
			<text class="ac-hud__l">1994 © PIXEL</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 注册免投币：CREDIT 默认 1，select → form
				phase: 'select',
				credit: 1,
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
				return ['--', 'WEAK', 'MID', 'FULL'][this.strength]
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

			pickMode() {
				this.phase = 'form'
			},

			// A 键：select=确认 / form=提交
			pressA() {
				if (this.phase === 'select') return this.pickMode()
				this.submit()
			},

			// B 键：form 退回 select
			pressB() {
				if (this.phase === 'form') this.phase = 'select'
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
				if (this.phase !== 'form') return
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
