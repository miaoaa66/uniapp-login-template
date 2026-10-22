<template>
	<view class="gb-page">
		<!-- 壳顶：返回 + 机型铭牌 -->
		<view class="gb-topbar">
			<view class="gb-topbar__back" @click="goBack">
				<view class="pxi pxi-back"></view>
				<text>BACK</text>
			</view>
			<text class="gb-topbar__t">PIXEL® DMG</text>
		</view>

		<!-- 机身 -->
		<view class="gb-shell">
			<!-- 屏幕外框 + 螺丝 -->
			<view class="gb-bezel">
				<view class="gb-screw gb-screw--tl"></view>
				<view class="gb-screw gb-screw--br"></view>

				<!-- LCD 屏幕 -->
				<view class="gb-screen" :class="{ 'gb-screen--form': phase === 'form' }">
					<!-- 开机动画：PIXEL® 落下 -->
					<block v-if="phase === 'boot'">
						<view class="gb-boot">
							<text class="gb-boot__logo">PIXEL®</text>
						</view>
					</block>

					<!-- 表单 -->
					<block v-else>
						<text class="gb-title">LOGIN</text>
						<text class="gb-lead">选择登录方式，按 A 确认</text>

						<!-- GB 纵向菜单：登录方式 -->
						<view class="gb-menu">
							<view v-for="(t, i) in tabs" :key="i" class="gb-menu__i" :class="{ on: tab === i }"
								@click="tab = i">
								<view class="gb-menu__cur"></view>
								<text>{{ t }}</text>
							</view>
						</view>
						<text class="gb-menu__hint">A CONFIRM</text>

						<!-- 菜单行 / 编辑态 两态字段 -->
						<view class="gb-form">
							<!-- 密码登录 -->
							<block v-if="tab === 0">
								<view class="gb-mrow" :class="{ edit: editing === 'account' }">
									<block v-if="editing !== 'account'">
										<text class="gb-mrow__label">账号</text>
										<view class="gb-mrow__dots"></view>
										<text class="gb-mrow__val" :class="{ empty: !account }"
											@click="editing = 'account'">{{ account || '未设定' }}</text>
									</block>
									<block v-else>
										<text class="gb-mrow__label">账号</text>
										<input class="gb-mrow__in" v-model="account" :focus="true"
											placeholder="未设定" placeholder-style="color:#8bac0f" maxlength="40"
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
								<view class="gb-mrow gb-mrow--link" @click="goForget">
									<text class="gb-mrow__label">忘记密码？</text>
									<view class="gb-mrow__dots"></view>
									<text class="gb-mrow__val">GO→</text>
								</view>
							</block>

							<!-- 手机验证码登录 -->
							<block v-else-if="tab === 1">
								<view class="gb-mrow" :class="{ edit: editing === 'phone' }">
									<block v-if="editing !== 'phone'">
										<text class="gb-mrow__label">手机号</text>
										<view class="gb-mrow__dots"></view>
										<text class="gb-mrow__val" :class="{ empty: !phone }"
											@click="editing = 'phone'">{{ phone || '未设定' }}</text>
									</block>
									<block v-else>
										<text class="gb-mrow__label">手机号</text>
										<input class="gb-mrow__in gb-mrow__in--en" v-model="phone" type="number" :focus="true"
											placeholder="未设定" placeholder-style="color:#8bac0f" maxlength="11"
											@blur="editing = ''" />
										<view class="gb-caret"></view>
									</block>
								</view>
								<view class="gb-mrow" :class="{ edit: editing === 'smsCode' }">
									<block v-if="editing !== 'smsCode'">
										<text class="gb-mrow__label">验证码</text>
										<view class="gb-mrow__dots"></view>
										<text class="gb-mrow__val gb-send" :class="{ off: smsLeft > 0 }"
											@click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : 'SEND' }}</text>
									</block>
									<block v-else>
										<text class="gb-mrow__label">验证码</text>
										<input class="gb-mrow__in gb-mrow__in--en" v-model="smsCode" type="number" :focus="true"
											placeholder="未设定" placeholder-style="color:#8bac0f" maxlength="6"
											@blur="editing = ''" />
										<view class="gb-caret"></view>
									</block>
								</view>
							</block>

							<!-- 邮箱验证码登录 -->
							<block v-else>
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
											@click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : 'SEND' }}</text>
									</block>
									<block v-else>
										<text class="gb-mrow__label">验证码</text>
										<input class="gb-mrow__in gb-mrow__in--en" v-model="mailCode" type="number" :focus="true"
											placeholder="未设定" placeholder-style="color:#8bac0f" maxlength="6"
											@blur="editing = ''" />
										<view class="gb-caret"></view>
									</block>
								</view>
							</block>

							<!-- 协议 -->
							<view class="gb-agree" @click="agreed = !agreed">
								<view class="gb-check" :class="{ on: agreed }"></view>
								<text>同意<text class="gb-agree__lk">《用户协议》</text>与<text class="gb-agree__lk">《隐私政策》</text></text>
							</view>

							<view class="gb-links">
								<text class="gb-links__lk" @click="goRegister">REGIST?</text>
								<text class="gb-links__bar">|</text>
								<text class="gb-links__lk" @click="goForget">FORGET?</text>
							</view>
						</view>
					</block>
				</view>
			</view>

			<!-- 实体键区：D-pad + A/B -->
			<view class="gb-keys">
				<view class="gb-dpad">
					<view class="gb-dpad__h"></view>
					<view class="gb-dpad__v"></view>
					<view class="gb-dpad__c"></view>
				</view>
				<view class="gb-ab">
					<view class="gb-abtn gb-abtn--b" @click="onB"><text>B</text></view>
					<view class="gb-abtn gb-abtn--a" @click="onA"><text>A</text></view>
				</view>
			</view>

			<!-- 壳底铭牌 + 扬声器 -->
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
				phase: 'boot', // boot | form
				editing: '',   // 当前编辑态字段
				tabs: ['密码登录', '手机验证码', '邮箱验证码'],
				tab: 0,
				agreed: false,
				showPwd: false,
				account: '',
				password: '',
				phone: '',
				smsCode: '',
				email: '',
				mailCode: '',
				smsLeft: 0,
				mailLeft: 0,
				timer: null
			}
		},
		computed: {
			pwdMask() {
				if (!this.password) return '未设定'
				return this.showPwd ? this.password : '••••••'
			}
		},
		onLoad(options) {
			// 演示参数：?tab=1&demo=1
			if (options && options.tab) this.tab = Number(options.tab) || 0
			if (options && options.demo) {
				this.fillDemo()
				this.phase = 'form'
			}
			// 开机动画：落字 → 停顿 → 闪屏进入表单
			setTimeout(() => {
				this.phase = 'form'
			}, 1800)
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			// A 键：提交
			onA() {
				this.submit()
			},

			// B 键：切换密码显隐
			onB() {
				this.showPwd = !this.showPwd
			},

			fillDemo() {
				this.agreed = true
				this.account = 'player@pixel.id'
				this.password = '1up-1983'
				this.phone = '13800001234'
				this.email = 'player@pixel.id'
				this.smsCode = '1983'
				this.mailCode = '1983'
			},

			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			isPhone(v) {
				return /^1\d{10}$/.test(v)
			},

			isEmail(v) {
				return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v)
			},

			sendCode(type) {
				if (type === 'sms') {
					if (this.smsLeft > 0) return
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
					this.smsLeft = 60
				} else {
					if (this.mailLeft > 0) return
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					this.mailLeft = 60
				}
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发送')
				this.startTimer()
			},

			startTimer() {
				if (this.timer) return
				this.timer = setInterval(() => {
					if (this.smsLeft > 0) this.smsLeft--
					if (this.mailLeft > 0) this.mailLeft--
					if (this.smsLeft <= 0 && this.mailLeft <= 0) this.clearTimer()
				}, 1000)
			},

			clearTimer() {
				if (this.timer) {
					clearInterval(this.timer)
					this.timer = null
				}
			},

			submit() {
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				if (this.tab === 0) {
					if (!this.account) return this.toast('请输入账号')
					if (!this.password) return this.toast('请输入登录密码')
				} else if (this.tab === 1) {
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
					if (!this.smsCode) return this.toast('请输入短信验证码')
				} else {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (!this.mailCode) return this.toast('请输入邮箱验证码')
				}
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goBack() {
				uni.navigateBack()
			},

			goRegister() {
				uni.navigateTo({
					url: '/pages/login93/register'
				})
			},

			goForget() {
				uni.navigateTo({
					url: '/pages/login93/forget'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
