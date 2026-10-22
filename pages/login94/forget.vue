<template>
	<view class="cr-scr">
		<!-- 顶部状态栏 -->
		<view class="cr-bar">
			<view class="cr-bar__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="cr-bar__t">PIXEL-OS // RECOVERY</text>
			<text class="cr-bar__r">STEP {{ step }}/2</text>
		</view>

		<view class="cr-scan"></view>

		<view class="cr-term">
			<!-- boot 日志 -->
			<view class="cr-boot">
				<text class="cr-boot__l cr-boot__l--1">PIXEL-OS v2.6 .......... OK</text>
				<text class="cr-boot__l cr-boot__l--2">RECOVERY TOOLS ......... OK</text>
				<text class="cr-boot__l cr-boot__l--3">READY <text class="cr-caret">▉</text></text>
			</view>

			<!-- 成功反白大字 -->
			<view v-if="restored" class="cr-restored">
				<text>ACCESS RESTORED</text>
			</view>

			<block v-else>
				<!-- 第一屏：IDENTIFY -->
				<block v-if="phase === 'identify'">
					<text class="cr-h cr-h--d1">RECOVERY://IDENTIFY</text>

					<view class="cr-lines cr-lines--d2">
						<view class="cr-line">
							<text class="cr-line__k">MAIL&gt;</text>
							<input class="cr-line__in" v-model="email" placeholder="REGISTERED MAIL" placeholder-style="color:#2b4a44" maxlength="40" />
							<text class="cr-send" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '[SEND]' }}</text>
						</view>

						<!-- 6 格终端方块验证码 -->
						<view class="cr-cells">
							<input v-for="(c, i) in cells" :key="i" class="cr-cell" :class="{ on: !!c }"
								v-model="cells[i]" type="number" maxlength="1" placeholder=" " placeholder-style="color:#12211d"
								:focus="focusIndex === i" @input="onCell(i, $event)" />
						</view>
						<text class="cr-tip">TYPE 6-DIGIT CODE</text>
					</view>

					<view class="cr-exec" :class="{ glitch: glitch }" @click="nextStep">
						<text>[ NEXT &gt; ]</text>
					</view>
				</block>

				<!-- 第二屏：NEW PASSPHRASE -->
				<block v-else>
					<text class="cr-h">RECOVERY://NEW PASSPHRASE</text>

					<view class="cr-lines">
						<view class="cr-line">
							<text class="cr-line__k">PASS&gt;</text>
							<input class="cr-line__in" v-model="password" :password="!showPwd" placeholder="8-20 CHARS" placeholder-style="color:#2b4a44" maxlength="20" />
							<view class="cr-line__ext" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
						<view class="cr-line">
							<text class="cr-line__k">CONF&gt;</text>
							<input class="cr-line__in" v-model="password2" :password="!showPwd" placeholder="REPEAT" placeholder-style="color:#2b4a44" maxlength="20" />
						</view>
					</view>

					<view class="cr-exec" :class="{ glitch: glitch }" @click="exec">
						<text>[ EXECUTE ]</text>
					</view>
				</block>
			</block>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				step: 1,
				phase: 'identify', // identify | reset
				showPwd: false,
				email: '',
				cells: ['', '', '', '', '', ''],
				focusIndex: 0,
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null,
				glitch: false,
				restored: false
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

			// 第一步局部校验后进入第二屏
			nextStep() {
				if (this.glitch) return
				this.glitch = true
				setTimeout(() => { this.glitch = false }, 220)
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
				this.step = 2
				this.phase = 'reset'
			},

			exec() {
				if (this.glitch) return
				this.glitch = true
				setTimeout(() => {
					this.glitch = false
					this.submit()
				}, 220)
			},

			// 保持原校验顺序
			submit() {
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
				if (this.password.length < 8) return this.toast('新密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				// TODO: 调用后端重置密码接口
				this.restored = true
				this.toast('密码已重置')
			},

			goBack() {
				uni.navigateBack()
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
