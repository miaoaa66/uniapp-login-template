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

				<!-- LCD 屏幕：存档命名 -->
				<view class="gb-screen gb-screen--form">
					<text class="gb-title">REGIST</text>
					<text class="gb-lead">创建你的存档，开始新冒险</text>

					<!-- 独立大格：昵称（编辑态常驻） -->
					<view class="gb-name">
						<input class="gb-name__in" v-model="nickname" placeholder="PLAYER-?"
							placeholder-style="color:#8bac0f" maxlength="12" />
						<text class="gb-name__tag">NAME?</text>
					</view>

					<!-- 菜单行 / 编辑态 两态字段 -->
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
						<view class="gb-mrow" :class="{ edit: editing === 'mailCode' }">
							<block v-if="editing !== 'mailCode'">
								<text class="gb-mrow__label">验证码</text>
								<view class="gb-mrow__dots"></view>
								<text class="gb-mrow__val gb-send" :class="{ off: mailLeft > 0 }"
									@click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : 'SEND' }}</text>
							</block>
							<block v-else>
								<text class="gb-mrow__label">验证码</text>
								<input class="gb-mrow__in gb-mrow__in--en" v-model="mailCode" type="number" :focus="true"
									placeholder="未设定" placeholder-style="color:#8bac0f" maxlength="6"
									@blur="editing = ''" />
								<view class="gb-caret"></view>
							</block>
						</view>
						<view class="gb-mrow" :class="{ edit: editing === 'password' }">
							<block v-if="editing !== 'password'">
								<text class="gb-mrow__label">密码</text>
								<view class="gb-mrow__dots"></view>
								<text class="gb-mrow__val" :class="{ empty: !password }"
									@click="editing = 'password'">{{ pwdMask }}</text>
							</block>
							<block v-else>
								<text class="gb-mrow__label">密码</text>
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

						<!-- POWER 三格 -->
						<view class="gb-power">
							<text class="gb-power__t">POWER</text>
							<view class="gb-power__cells">
								<view class="gb-power__c" :class="{ on: strength >= 1 }"></view>
								<view class="gb-power__c" :class="{ on: strength >= 2 }"></view>
								<view class="gb-power__c" :class="{ on: strength >= 3 }"></view>
							</view>
							<text class="gb-power__v">{{ strengthText }}</text>
						</view>

						<!-- 协议 -->
						<view class="gb-agree" @click="agreed = !agreed">
							<view class="gb-check" :class="{ on: agreed }"></view>
							<text>同意<text class="gb-agree__lk">《用户协议》</text>与<text class="gb-agree__lk">《隐私政策》</text></text>
						</view>

						<view class="gb-links">
							<text class="gb-links__lk" @click="goLogin">LOGIN?</text>
						</view>
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
				editing: '',
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
			// B 键：切换密码显隐
			onB() {
				this.showPwd = !this.showPwd
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
