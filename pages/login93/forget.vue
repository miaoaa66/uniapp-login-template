<template>
	<view class="gb-page">
		<view class="gb-topbar">
			<view class="gb-topbar__back" @click="goBack">
				<view class="pxi pxi-back"></view>
				<text>BACK</text>
			</view>
			<text class="gb-topbar__t">PIXEL® DMG</text>
		</view>

		<!-- 机身 -->
		<view class="gb-shell">
			<view class="gb-bezel">
				<view class="gb-screw gb-screw--tl"></view>
				<view class="gb-screw gb-screw--br"></view>

				<!-- LCD 屏幕：菜单式两步 -->
				<view class="gb-screen gb-screen--form">
					<text class="gb-title">RESET</text>
					<text class="gb-lead">通过邮箱验证码重设登录密码</text>

					<!-- STEP 1/2 反色条 -->
					<view class="gb-stepbar" @click="stepBack">
						<text>STEP {{ step }}/2 · {{ step === 1 ? 'MAIL CODE' : 'NEW PASSWORD' }}</text>
					</view>

					<!-- 第一步：邮箱 + 验证码 -->
					<block v-if="step === 1">
						<view class="gb-form">
							<view class="gb-mrow" :class="{ edit: editing === 'email' }">
								<block v-if="editing !== 'email'">
									<text class="gb-mrow__label">邮箱</text>
									<view class="gb-mrow__dots"></view>
									<text class="gb-mrow__val" :class="{ empty: !email }"
										@click="editing = 'email'">{{ email || '未设定' }}</text>
								</block>
								<block v-else>
									<text class="gb-mrow__label">邮箱</text>
									<input class="gb-mrow__in" v-model="email" :focus="true"
										placeholder="未设定" placeholder-style="color:#8bac0f" maxlength="40"
										@blur="editing = ''" />
									<view class="gb-caret"></view>
								</block>
							</view>

							<!-- SEND 小按钮行 -->
							<view class="gb-sendrow" :class="{ off: mailLeft > 0 }" @click="sendCode">
								<text>{{ mailLeft > 0 ? 'WAIT ' + mailLeft + 's' : 'SEND MAIL' }}</text>
							</view>
						</view>

						<!-- 6 个 GB 方块验证码 -->
						<view class="gb-cells">
							<input v-for="(c, i) in cells" :key="i" class="gb-cell" :class="{ fill: c }"
								v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i"
								placeholder="·" placeholder-style="color:#8bac0f"
								@input="onCell(i, $event)" @focus="focusIndex = i" />
						</view>
						<text class="gb-tip">输入 6 位验证码</text>
					</block>

					<!-- 第二步：新密码 -->
					<block v-else>
						<view class="gb-form">
							<view class="gb-mrow" :class="{ edit: editing === 'password' }">
								<block v-if="editing !== 'password'">
									<text class="gb-mrow__label">新密码</text>
									<view class="gb-mrow__dots"></view>
									<text class="gb-mrow__val" :class="{ empty: !password }"
										@click="editing = 'password'">{{ pwdMask }}</text>
								</block>
								<block v-else>
									<text class="gb-mrow__label">新密码</text>
									<input class="gb-mrow__in" v-model="password" :password="!showPwd" :focus="true"
										placeholder="未设定" placeholder-style="color:#8bac0f" maxlength="20"
										@blur="editing = ''" />
									<view class="gb-caret"></view>
								</block>
							</view>
							<view class="gb-mrow" :class="{ edit: editing === 'password2' }">
								<block v-if="editing !== 'password2'">
									<text class="gb-mrow__label">确认</text>
									<view class="gb-mrow__dots"></view>
									<text class="gb-mrow__val" :class="{ empty: !password2 }"
										@click="editing = 'password2'">{{ pwd2Mask }}</text>
								</block>
								<block v-else>
									<text class="gb-mrow__label">确认</text>
									<input class="gb-mrow__in" v-model="password2" :password="!showPwd" :focus="true"
										placeholder="未设定" placeholder-style="color:#8bac0f" maxlength="20"
										@blur="editing = ''" />
									<view class="gb-caret"></view>
								</block>
							</view>
						</view>
						<text class="gb-tip">按 A 键完成重置</text>
					</block>

					<view class="gb-links">
						<text class="gb-links__lk" @click="goLogin">LOGIN?</text>
					</view>
				</view>
			</view>

			<!-- 实体键区 -->
			<view class="gb-keys">
				<view class="gb-dpad">
					<view class="gb-dpad__h"></view>
					<view class="gb-dpad__v"></view>
					<view class="gb-dpad__c"></view>
				</view>
				<view class="gb-ab">
					<view class="gb-abtn gb-abtn--b" @click="onB"><text>B</text></view>
					<view class="gb-abtn gb-abtn--a" @click="submit"><text>A</text></view>
				</view>
			</view>

			<view class="gb-brandrow">
				<text class="gb-brandrow__t">DOT MATRIX WITH STEREO SOUND</text>
				<view class="gb-speaker">
					<view v-for="n in 3" :key="n" class="gb-speaker__i"></view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				step: 1,
				editing: '',
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
		computed: {
			pwdMask() {
				if (!this.password) return '未设定'
				return this.showPwd ? this.password : '••••••'
			},
			pwd2Mask() {
				if (!this.password2) return '未设定'
				return this.showPwd ? this.password2 : '••••••'
			}
		},
		onLoad(options) {
			if (options && options.demo) {
				this.email = 'player@pixel.id'
				this.cells = ['1', '9', '8', '3', '', '']
				this.password = '1up-1983'
				this.password2 = '1up-1983'
				// 密码已预填，直接落到第二步
				this.step = 2
			}
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			// B 键：第二步返回第一步
			onB() {
				if (this.step === 2) this.step = 1
			},

			// 反色条点击：第二步可退回
			stepBack() {
				if (this.step === 2) this.step = 1
			},

			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
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

			// A 键：分步提交，校验顺序与原版一致
			submit() {
				if (this.step === 1) {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
					this.step = 2
					return
				}
				if (this.password.length < 8) return this.toast('新密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				// TODO: 调用后端重置密码接口
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
