<template>
	<view class="page">
		<view class="status" :style="{ height: statusBar + 'px' }"></view>

		<view class="nav">
			<view class="back" @click="goBack"><view class="back-line"></view></view>
			<text class="nav-idx">SEC 01 / 05</text>
		</view>

		<view class="radar">
			<view class="radar-line-h"></view>
			<view class="radar-line-v"></view>
			<view class="radar-sweep"></view>
			<view class="radar-core">
				<text class="radar-v">{{ status }}</text>
				<text class="radar-h">CREW ACCESS</text>
			</view>
		</view>

		<view class="head">
			<text class="title">全息舷窗</text>
			<text class="lead">拖动滑块完成身份授权，放行进入主舱。</text>
		</view>

		<view class="slots">
			<view class="slot" :class="{ active: focus === 'account', error: !!err.account }">
				<text class="slot-tag">ACC</text>
				<view class="slot-main">
					<text class="slot-label">账号</text>
					<input class="input" v-model="account" :placeholder-style="phStyle" placeholder="手机号 / 邮箱 / 工号" maxlength="40"
						@focus="focus = 'account'" @blur="focus = ''" />
				</view>
			</view>
			<text v-if="err.account" class="err">{{ err.account }}</text>

			<view class="slot" :class="{ active: focus === 'password', error: !!err.password }">
				<text class="slot-tag">KEY</text>
				<view class="slot-main">
					<text class="slot-label">口令</text>
					<input class="input" v-model="password" :password="!showPwd" :placeholder-style="phStyle" placeholder="请输入登录口令" maxlength="20"
						@focus="focus = 'password'" @blur="focus = ''" />
					<text class="eye" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
				</view>
			</view>
			<text v-if="err.password" class="err">{{ err.password }}</text>

			<view class="row">
				<view class="check" @click="remember = !remember">
					<view class="check-box" :class="{ on: remember }"><view class="check-tick"></view></view>
					<text class="check-txt">记住此设备</text>
				</view>
				<text class="link link-soft" @click="goForget">忘记口令</text>
			</view>

			<view class="slider-wrap">
				<view class="track" @touchstart="onTouchStart" @touchmove="onTouchMove" @touchend="onTouchEnd">
					<view class="track-fill" :style="{ width: knobX + 'px' }"></view>
					<text class="track-txt" :class="{ gone: dragging || knobX > 4 }">{{ submitting ? 'AUTHORIZING' : 'SLIDE TO AUTHORIZE' }}</text>
					<view class="knob" :style="{ transform: 'translateX(' + knobX + 'px)' }">
						<text class="knob-arrow">▸</text>
					</view>
				</view>
			</view>

			<view class="check" style="margin-top: 28rpx;" @click="agreed = !agreed">
				<view class="check-box" :class="{ on: agreed }"><view class="check-tick"></view></view>
				<text class="check-txt">我已阅读并同意 <text class="link">《用户协议》</text> 与 <text class="link">《隐私政策》</text></text>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text>还没有账号</text>
				<text class="link" @click="goRegister">创建账号</text>
				<text class="link" style="margin-left: 24rpx;" @click="goPhoneLogin">验证码登录</text>
			</view>
			<text class="copy">© 2026 HOLO DECK · CLEARANCE L2</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				statusBar: 0,
				status: 'IDLE',
				focus: '',
				showPwd: false,
				remember: false,
				agreed: false,
				submitting: false,
				phStyle: 'color:rgba(94,140,136,0.8);font-size:15px;',
				account: '',
				password: '',
				err: {},
				knobX: 0,
				startX: 0,
				baseX: 0,
				maxX: 0,
				trackPx: 0,
				knobPx: 0,
				dragging: false
			}
		},
		onLoad() {
			const info = uni.getSystemInfoSync()
			this.statusBar = info.statusBarHeight || 0
		},
		onReady() {
			uni.createSelectorQuery().select('.track').boundingClientRect(rect => {
				if (!rect) return
				this.trackPx = rect.width
				this.knobPx = uni.upx2px(92) || 46
				this.maxX = Math.max(0, this.trackPx - this.knobPx - 4)
			}).exec()
		},
		methods: {
			toast(title) {
				uni.showToast({ title, icon: 'none' })
			},

			isValidAccount(v) {
				if (!v) return false
				if (v.indexOf('@') > -1) return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v)
				return /^1\d{10}$/.test(v)
			},

			onTouchStart(e) {
				if (this.submitting) return
				this.dragging = true
				this.startX = e.touches[0].clientX
				this.baseX = this.knobX
			},

			onTouchMove(e) {
				if (!this.dragging) return
				const dx = e.touches[0].clientX - this.startX
				this.knobX = Math.min(this.maxX, Math.max(0, this.baseX + dx))
			},

			onTouchEnd() {
				if (!this.dragging) return
				this.dragging = false
				if (this.knobX >= this.maxX - 6) {
					this.knobX = this.maxX
					this.submit()
				} else {
					this.knobX = 0
				}
			},

			validate() {
				const e = {}
				if (!this.account) e.account = '请输入账号'
				else if (!this.isValidAccount(this.account)) e.account = '账号格式不正确'
				if (!this.password) e.password = '请输入登录口令'
				else if (this.password.length < 6) e.password = '口令不少于 6 位'
				this.err = e
				return Object.keys(e).length === 0
			},

			submit() {
				if (this.submitting) return
				if (!this.agreed) {
					this.knobX = 0
					return this.toast('请先阅读并同意用户协议')
				}
				if (!this.validate()) {
					this.knobX = 0
					return
				}
				this.submitting = true
				this.status = 'AUTH'
				// TODO: 调用后端登录接口，成功后跳转首页
				setTimeout(() => {
					this.submitting = false
					this.status = 'IDLE'
					this.knobX = 0
					this.toast('授权成功')
				}, 700)
			},

			goBack() {
				const pages = getCurrentPages()
				if (pages.length > 1) uni.navigateBack()
				else uni.reLaunch({ url: '/pages/index' })
			},

			goRegister() {
				uni.navigateTo({ url: '/pages/login129/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login129/forget' })
			},

			goPhoneLogin() {
				uni.navigateTo({ url: '/pages/login129/phone-login' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
