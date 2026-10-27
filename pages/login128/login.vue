<template>
	<view class="page">
		<view class="blob blob-1"></view>
		<view class="blob blob-2"></view>

		<view class="content">
			<view class="tray pop pop-1">
				<view class="tray-disc">
					<view class="tray-jelly"></view>
				</view>
				<view class="tray-shadow"></view>
				<view class="tray-name">果冻工坊</view>
				<view class="tray-sub">JELLY TRAY</view>
			</view>

			<view class="head pop pop-2">
				<view class="head-title">按住那颗软糖</view>
				<view class="head-sub">长按按钮充能，果冻浆涨满即自动登录。</view>
			</view>

			<view class="chips pop pop-3">
				<view class="chip on">密码</view>
				<view class="chip" @click="goPhone">短信</view>
				<view class="chip" @click="goEmail">邮箱</view>
			</view>

			<view class="card pop pop-3">
				<view class="field">
					<text class="field-label">账号</text>
					<view class="field-box" :class="{ active: focusField === 'account' }">
						<input class="input" v-model="account" placeholder="手机号 / 邮箱" placeholder-class="ph" maxlength="40"
							@focus="focusField = 'account'" @blur="focusField = ''" />
					</view>
				</view>

				<view class="field">
					<text class="field-label">密码</text>
					<view class="field-box" :class="{ active: focusField === 'password' }">
						<input class="input" v-model="password" :password="!showPassword" placeholder="请输入登录密码" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password'" @blur="focusField = ''" />
						<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐藏' : '显示' }}</text>
					</view>
				</view>

				<view class="row-between">
					<view class="remember" @click="remember = !remember">
						<view class="checkbox" :class="{ on: remember }"></view>
						<text>记住我</text>
					</view>
					<text class="link" @click="goForget">软糖化了？（找回密码）</text>
				</view>

				<view class="hold-btn" :class="{ pressing: charging, charged: charged }"
					@touchstart.stop.prevent="holdStart" @touchend.stop.prevent="holdEnd" @touchcancel.stop.prevent="holdEnd">
					<view class="hold-fill" :style="{ width: charge + '%' }"></view>
					<text class="hold-text" :class="{ on: charge > 55 }">{{ charge > 0 ? '充能中 ' + charge + '%' : '长按充能登录' }}</text>
				</view>
				<view class="hold-tip">{{ holdTip }}</view>

				<view class="foot-link">
					还没有账号？<text class="link" @click="goRegister">去捏一个</text>
				</view>

				<view class="agreement">
					<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
					<text>我已阅读并同意<text class="link">《用户协议》</text>与<text class="link">《隐私政策》</text></text>
				</view>
			</view>

			<view class="foot-note pop pop-4">© 2026 JELLY TRAY · 果冻工坊</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				account: '',
				password: '',
				showPassword: false,
				remember: true,
				agreed: false,
				focusField: '',
				charge: 0,
				charging: false,
				charged: false,
				chargeTimer: null,
				holdTip: ''
			}
		},
		onUnload() {
			this.stopCharge()
		},
		methods: {
			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			/* ---------- 长按充能 ---------- */
			holdStart() {
				if (this.charged) return
				this.holdTip = ''
				this.charging = true
				this.stopCharge()
				this.chargeTimer = setInterval(() => {
					this.charge += 4
					if (this.charge >= 100) {
						this.charge = 100
						this.stopCharge()
						this.charged = true
						this.charging = false
						setTimeout(() => {
							this.charged = false
							this.charge = 0
						}, 420)
						this.submit()
					}
				}, 36) // 约 0.9s 充满
			},

			holdEnd() {
				if (this.charged) return
				this.charging = false
				if (this.charge > 0 && this.charge < 100) {
					this.holdTip = '差一点点，再按久一点~'
				}
				this.charge = 0
				this.stopCharge()
			},

			stopCharge() {
				if (this.chargeTimer) {
					clearInterval(this.chargeTimer)
					this.chargeTimer = null
				}
			},

			submit() {
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				if (!this.account) return this.toast('请输入账号')
				if (!this.password) return this.toast('请输入登录密码')
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('充能完成 · 登录成功')
			},

			goRegister() {
				uni.navigateTo({
					url: '/pages/login128/register'
				})
			},

			goForget() {
				uni.navigateTo({
					url: '/pages/login128/forget'
				})
			},

			goPhone() {
				uni.navigateTo({
					url: '/pages/login128/phone-login'
				})
			},

			goEmail() {
				uni.navigateTo({
					url: '/pages/login128/email-login'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
