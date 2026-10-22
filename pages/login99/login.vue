<template>
	<view class="tk-page">
		<!-- 顶部窄条：返回 + 标题 -->
		<view class="tk-top">
			<view class="tk-back" @click="goBack"><view class="pxi pxi-back"></view></view>
			<text class="tk-top__t">地铁 · 单程票</text>
		</view>

		<!-- 横向大车票 -->
		<view class="tk-ticket">
			<!-- 左侧票根：撕口选项 = 登录方式切换 -->
			<view class="tk-stub">
				<text class="tk-stub__tag">METRO</text>
				<view v-for="(item, i) in tabs" :key="i" class="tk-tear"
					:class="{ on: tab === i, tear: leaving === i }" @click="pick(i)">
					<text class="tk-tear__t">{{ item }}</text>
				</view>
				<view class="tk-stub__agree" @click="agreed = !agreed">
					<view class="tk-dot" :class="{ on: agreed }"></view>
					<text class="tk-stub__agree-t">已阅并同意《用户协议》与《隐私政策》</text>
				</view>
			</view>

			<!-- 打孔虚线分隔 -->
			<view class="tk-holes">
				<view v-for="n in 6" :key="n" class="tk-hole"></view>
			</view>

			<!-- 右侧主票 -->
			<view class="tk-main">
				<view class="tk-route">
					<text class="tk-route__a">始发站 PIXEL</text>
					<view class="tk-route__arrow"></view>
					<text class="tk-route__b">终点站 登录</text>
				</view>

				<view class="tk-fields">
					<block v-if="tab === 0">
						<view class="tk-field">
							<text class="tk-field__label">账号</text>
							<input class="tk-field__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#93a3b8" maxlength="40" />
						</view>
						<view class="tk-field">
							<text class="tk-field__label">密码</text>
							<input class="tk-field__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#93a3b8" maxlength="20" />
							<view class="tk-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
					</block>
					<block v-else-if="tab === 1">
						<view class="tk-field">
							<text class="tk-field__label">手机</text>
							<input class="tk-field__in" v-model="phone" type="number" placeholder="11 位手机号" placeholder-style="color:#93a3b8" maxlength="11" />
						</view>
						<view class="tk-field">
							<text class="tk-field__label">印文</text>
							<input class="tk-field__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#93a3b8" maxlength="6" />
							<text class="tk-get" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</text>
						</view>
					</block>
					<block v-else>
						<view class="tk-field">
							<text class="tk-field__label">邮箱</text>
							<input class="tk-field__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#93a3b8" maxlength="40" />
						</view>
						<view class="tk-field">
							<text class="tk-field__label">印文</text>
							<input class="tk-field__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#93a3b8" maxlength="6" />
							<text class="tk-get" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</text>
						</view>
					</block>
				</view>

				<view class="tk-price">
					<text class="tk-price__v">¥ 0.00</text>
					<text class="tk-price__l">单程 · 限乘一次</text>
				</view>

				<!-- 条码区 -->
				<view class="tk-bars">
					<view v-for="n in 24" :key="n" class="tk-bar" :class="'tk-bar--' + (n % 5)"></view>
				</view>

				<view class="tk-btn" @click="submit"><text class="tk-btn__t">验 票</text></view>

				<view class="tk-links">
					<text class="tk-links__l" @click="goForget">补票窗口 · 忘记密码</text>
					<text class="tk-links__l" @click="goRegister">没有车票？换乘登记</text>
				</view>

				<!-- 已验票印章 -->
				<view class="tk-stamp" v-if="stamped"><text class="tk-stamp__t">已验票</text></view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabs: ["密码", "短信", "邮箱"],
				tab: 0,
				leaving: null,
				leavingTimer: null,
				stamped: false,
				stampTimer: null,
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
		onLoad(options) {
			// 演示参数：?tab=1&demo=1
			if (options && options.tab) this.tab = Number(options.tab) || 0
			if (options && options.demo) this.fillDemo()
		},
		onUnload() {
			this.clearTimer()
			if (this.leavingTimer) clearTimeout(this.leavingTimer)
			if (this.stampTimer) clearTimeout(this.stampTimer)
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

			// 撕口切换：旧选中项做一次撕离动画
			pick(i) {
				if (i === this.tab) return
				this.leaving = this.tab
				this.tab = i
				if (this.leavingTimer) clearTimeout(this.leavingTimer)
				this.leavingTimer = setTimeout(() => { this.leaving = null }, 320)
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
				// 盖章后再提示
				this.stamped = true
				if (this.stampTimer) clearTimeout(this.stampTimer)
				this.stampTimer = setTimeout(() => {
					// TODO: 调用后端登录接口，成功后跳转首页
					this.toast('登录成功')
				}, 500)
			},

			goBack() {
				uni.navigateBack()
			},

			goRegister() {
				uni.navigateTo({ url: '/pages/login99/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login99/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
