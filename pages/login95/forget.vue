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
			<text class="ac-marquee__t">CONTINUE?</text>
			<view class="ac-bulbs">
				<view v-for="n in 4" :key="'r'+n" class="ac-bulb" :class="n % 2 ? 'ac-bulb--b' : 'ac-bulb--a'"></view>
			</view>
		</view>

		<!-- 投币口（装饰） -->
		<view class="ac-slotrow">
			<text class="ac-slotrow__t">COIN SLOT</text>
			<view class="ac-slot"></view>
		</view>

		<!-- 屏幕 -->
		<view class="ac-screen" @click="screenTap">
			<!-- GAME OVER? → CONTINUE? -->
			<block v-if="phase === 'over'">
				<text class="ac-over">GAME OVER?</text>
				<text class="ac-cont">CONTINUE?</text>
				<text class="ac-hint">TAP SCREEN / A BUTTON</text>
			</block>

			<!-- form：邮箱 + 验证码 + 新密码 同屏 -->
			<block v-else-if="phase === 'form'">
				<view class="ac-f">
					<text class="ac-f__k">MAIL ADDR</text>
					<input class="ac-f__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#5a5470" maxlength="40" />
					<text class="ac-f__send" :class="{ off: mailLeft > 0 }" @click.stop="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : 'SEND' }}</text>
				</view>

				<view class="ac-cells">
					<input v-for="(c, i) in cells" :key="i" class="ac-cell" :class="{ on: !!c }"
						v-model="cells[i]" type="number" maxlength="1" placeholder=" " placeholder-style="color:#2a2438"
						:focus="focusIndex === i" @input="onCell(i, $event)" />
				</view>
				<text class="ac-celltip">6-DIGIT MAIL CODE</text>

				<view class="ac-f">
					<text class="ac-f__k">NEW PASSWORD</text>
					<input class="ac-f__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#5a5470" maxlength="20" />
					<view class="ac-f__eye" @click.stop="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
				</view>

				<view class="ac-f">
					<text class="ac-f__k">REPEAT</text>
					<input class="ac-f__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#5a5470" maxlength="20" />
				</view>

				<text class="ac-push">PUSH START</text>
			</block>

			<!-- 提交成功 -->
			<block v-else>
				<text class="ac-ok">CONTINUE... OK</text>
				<text class="ac-hint">PASSWORD RESTORED</text>
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
				// over → form → win
				phase: 'over',
				credit: 1,
				step: 1,
				showPwd: false,
				email: '',
				cells: ['', '', '', '', '', ''],
				focusIndex: 0,
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null
			}
		},
		onLoad(options) {
			if (options && options.demo) {
				this.email = 'player@pixel.id'
				this.cells = ['1', '9', '8', '3', '', '']
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

			// 输入后自动跳下一格
			onCell(i, e) {
				const v = (e.detail && e.detail.value) || ''
				if (v && i < 5) this.focusIndex = i + 1
			},

			screenTap() {
				if (this.phase === 'over') this.continueGame()
			},

			continueGame() {
				if (this.phase !== 'over') return
				this.step = 2
				this.phase = 'form'
			},

			// A 键：over=继续 / form=提交
			pressA() {
				if (this.phase === 'over') return this.continueGame()
				if (this.phase === 'form') this.submit()
			},

			// B 键：form 退回 over
			pressB() {
				if (this.phase === 'form') {
					this.step = 1
					this.phase = 'over'
				}
			},

			sendCode() {
				if (this.mailLeft > 0) return
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				this.mailLeft = 60
				this.focusIndex = 0
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

			// 保持原校验顺序
			submit() {
				if (this.phase !== 'form') return
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
				if (this.password.length < 8) return this.toast('新密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				// TODO: 调用后端重置密码接口
				this.phase = 'win'
				this.toast('密码已重置')
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
