<template>
	<view class="bk-page">
		<!-- 顶部：积木块返回键 + 标题 -->
		<view class="bk-top">
			<view class="bk-back" @click="goBack">
				<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view></view>
				<view class="pxi pxi-back"></view>
			</view>
			<view class="bk-top__t">
				<text class="bk-top__cn">拼出你的账号</text>
				<text class="bk-top__en">BRICK LOGIN</text>
			</view>
		</view>

		<!-- 模式切换：3 块 2x1 小积木 -->
		<view class="bk-modes">
			<view v-for="(m, i) in tabs" :key="i" class="bk-mode" :class="['bk-mode--' + i, { on: tab === i }]" @click="tab = i">
				<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view></view>
				<text>{{ m }}</text>
			</view>
		</view>

		<!-- 积木塔表单：载入逐块落位，聚焦抬起 -->
		<view>
			<block v-if="tab === 0">
				<view class="bk-brick bk-brick--red" :class="{ up: focusField === 'account' }" style="animation-delay: .05s;">
					<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
					<input class="bk-brick__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#ffb9bd" maxlength="40" @focus="focusField = 'account'" @blur="focusField = ''" />
				</view>
				<view class="bk-brick bk-brick--yellow" :class="{ up: focusField === 'password' }" style="animation-delay: .13s;">
					<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
					<input class="bk-brick__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#8a7400" maxlength="20" @focus="focusField = 'password'" @blur="focusField = ''" />
					<view class="bk-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
				</view>
				<view class="bk-row"><text class="bk-row__link" @click="goForget">少了一块？拆除重建 →</text></view>
			</block>
			<block v-else-if="tab === 1">
				<view class="bk-brick bk-brick--blue" :class="{ up: focusField === 'phone' }" style="animation-delay: .05s;">
					<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
					<input class="bk-brick__in" v-model="phone" type="number" placeholder="11 位手机号" placeholder-style="color:#9fc4e8" maxlength="11" @focus="focusField = 'phone'" @blur="focusField = ''" />
				</view>
				<view class="bk-brick bk-brick--green" :class="{ up: focusField === 'smsCode' }" style="animation-delay: .13s;">
					<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
					<input class="bk-brick__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#a9dcb4" maxlength="6" @focus="focusField = 'smsCode'" @blur="focusField = ''" />
					<view class="bk-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</view>
				</view>
			</block>
			<block v-else>
				<view class="bk-brick bk-brick--orange" :class="{ up: focusField === 'email' }" style="animation-delay: .05s;">
					<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
					<input class="bk-brick__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#ffd9b3" maxlength="40" @focus="focusField = 'email'" @blur="focusField = ''" />
				</view>
				<view class="bk-brick bk-brick--green" :class="{ up: focusField === 'mailCode' }" style="animation-delay: .13s;">
					<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
					<input class="bk-brick__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#a9dcb4" maxlength="6" @focus="focusField = 'mailCode'" @blur="focusField = ''" />
					<view class="bk-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
				</view>
			</block>

			<!-- 提交：超宽绿色基板 -->
			<view class="bk-base" style="animation-delay: .21s;" @click="submit">
				<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
				<text>开 始 拼 装</text>
			</view>

			<view class="bk-foot">
				<text>还没有积木？</text>
				<text class="bk-foot__link" @click="goRegister">立即注册</text>
			</view>

			<view class="bk-agree" @click="agreed = !agreed">
				<view class="bk-check" :class="{ on: agreed }"></view>
				<text>我已阅读并同意<text class="bk-link">《用户协议》</text>与<text class="bk-link">《隐私政策》</text></text>
			</view>
		</view>

		<!-- 底部拼装底板 -->
		<view class="bk-floor"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabs: ["密码登录","手机验证码","邮箱验证码"],
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
				timer: null,
				focusField: ''
			}
		},
		onLoad(options) {
			// 演示参数：?tab=1&demo=1
			if (options && options.tab) this.tab = Number(options.tab) || 0
			if (options && options.demo) this.fillDemo()
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
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
				uni.showToast({ title, icon: 'none' })
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
				uni.navigateTo({ url: '/pages/login104/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login104/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
