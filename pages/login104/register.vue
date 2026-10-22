<template>
	<view class="bk-page">
		<!-- 顶部：积木块返回键 + 说明书标题 -->
		<view class="bk-top">
			<view class="bk-back" @click="goBack">
				<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view></view>
				<view class="pxi pxi-back"></view>
			</view>
			<view class="bk-top__t">
				<text class="bk-top__cn">拼搭说明书</text>
				<text class="bk-top__en">ASSEMBLY MANUAL {{ rstep }}/3</text>
			</view>
		</view>

		<!-- 3 步圆点 -->
		<view class="bk-stepsrow">
			<view class="bk-dot" :class="{ done: rstep > 1, cur: rstep === 1 }"><text>1</text></view>
			<view class="bk-dotline" :class="{ on: rstep > 1 }"></view>
			<view class="bk-dot" :class="{ done: rstep > 2, cur: rstep === 2 }"><text>2</text></view>
			<view class="bk-dotline" :class="{ on: rstep > 2 }"></view>
			<view class="bk-dot" :class="{ done: rstep > 3, cur: rstep === 3 }"><text>3</text></view>
		</view>

		<view class="bk-manual">
			<!-- 左侧成品图：已完成步骤打勾 -->
			<view class="bk-result">
				<text class="bk-result__t">成品图</text>
				<view class="bk-mini bk-mini--1" :class="{ done: rstep > 1 }">
					<view class="bk-mini__studs"><view class="bk-ministud"></view><view class="bk-ministud"></view></view>
				</view>
				<view class="bk-mini bk-mini--2" :class="{ done: rstep > 2 }">
					<view class="bk-mini__studs"><view class="bk-ministud"></view><view class="bk-ministud"></view></view>
				</view>
				<view class="bk-mini bk-mini--3" :class="{ done: rstep > 3 }">
					<view class="bk-mini__studs"><view class="bk-ministud"></view><view class="bk-ministud"></view></view>
				</view>
			</view>

			<!-- 右侧搭建区 -->
			<view class="bk-build">
				<block v-if="rstep === 1">
					<view class="bk-brick bk-brick--red" :class="{ up: focusField === 'nickname' }" style="animation-delay: .05s;">
						<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
						<input class="bk-brick__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#ffb9bd" maxlength="12" @focus="focusField = 'nickname'" @blur="focusField = ''" />
					</view>
					<view class="bk-next" @click="nextStep">
						<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
						<text>下一步 · 拼联系方式</text>
					</view>
				</block>
				<block v-else-if="rstep === 2">
					<view class="bk-brick bk-brick--orange" :class="{ up: focusField === 'email' }" style="animation-delay: .05s;">
						<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
						<input class="bk-brick__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#ffd9b3" maxlength="40" @focus="focusField = 'email'" @blur="focusField = ''" />
					</view>
					<view class="bk-brick bk-brick--green" :class="{ up: focusField === 'mailCode' }" style="animation-delay: .13s;">
						<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
						<input class="bk-brick__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#a9dcb4" maxlength="6" @focus="focusField = 'mailCode'" @blur="focusField = ''" />
						<view class="bk-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>
					<view class="bk-next" @click="nextStep">
						<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
						<text>下一步 · 拼密码</text>
					</view>
				</block>
				<block v-else>
					<view class="bk-brick bk-brick--yellow" :class="{ up: focusField === 'password' }" style="animation-delay: .05s;">
						<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
						<input class="bk-brick__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#8a7400" maxlength="20" @focus="focusField = 'password'" @blur="focusField = ''" />
						<view class="bk-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>
					<view class="bk-brick bk-brick--red" :class="{ up: focusField === 'password2' }" style="animation-delay: .13s;">
						<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
						<input class="bk-brick__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#ffb9bd" maxlength="20" @focus="focusField = 'password2'" @blur="focusField = ''" />
					</view>
					<view class="bk-power">
						<text class="bk-power__t">强度</text>
						<view class="bk-power__stud" :class="{ on: strength >= 1 }"></view>
						<view class="bk-power__stud" :class="{ on: strength >= 2 }"></view>
						<view class="bk-power__stud" :class="{ on: strength >= 3 }"></view>
						<text>{{ strengthText }}</text>
					</view>
					<view class="bk-base" @click="submit">
						<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
						<text>完 成 拼 装</text>
					</view>
				</block>

				<view class="bk-foot">
					<text>已有套装？</text>
					<text class="bk-foot__link" @click="goLogin">返回登录</text>
				</view>

				<view class="bk-agree" @click="agreed = !agreed">
					<view class="bk-check" :class="{ on: agreed }"></view>
					<text>我已阅读并同意<text class="bk-link">《用户协议》</text>与<text class="bk-link">《隐私政策》</text></text>
				</view>
			</view>
		</view>

		<view class="bk-floor"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				rstep: 1,
				agreed: false,
				showPwd: false,
				nickname: '',
				email: '',
				mailCode: '',
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null,
				focusField: ''
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
			// 分步校验：通过才进入下一步，最终提交仍校验全部字段
			nextStep() {
				if (this.rstep === 1) {
					if (!this.nickname) return this.toast('请输入昵称')
					this.rstep = 2
				} else if (this.rstep === 2) {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (!this.mailCode) return this.toast('请输入邮箱验证码')
					this.rstep = 3
				}
			},

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
