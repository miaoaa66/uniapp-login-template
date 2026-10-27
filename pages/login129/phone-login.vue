<template>
	<view class="page">
		<view class="status" :style="{ height: statusBar + 'px' }"></view>

		<view class="nav">
			<view class="back" @click="goBack"><view class="back-line"></view></view>
			<text class="nav-idx">CORE 02 / 05</text>
		</view>

		<view class="core-head">
			<view class="crest"></view>
			<view class="core-txt">
				<text class="core-title">HOLO CORE // 全息中枢</text>
				<text class="core-sub">SMS CHANNEL · TEL LINK</text>
			</view>
			<view class="beacon" :class="{ busy: submitting }">
				<view class="beacon-dot"></view>
				<text>{{ submitting ? 'BUSY' : 'IDLE' }}</text>
			</view>
		</view>

		<view class="orbital">
			<view class="orb-knob" :style="{ transform: 'translateX(' + orbIndex * 100 + '%)' }"></view>
			<view v-for="(s, i) in orbSegs" :key="i" class="orb-seg" :class="{ on: i === orbIndex }" @click="goSeg(i)">{{ s }}</view>
		</view>

		<view class="head">
			<text class="title">短信波段验证</text>
			<text class="lead">六格能量将随验证码逐格点亮，波段对准即放行。</text>
		</view>

		<view class="panel">
			<view class="slot" :class="{ active: focus === 'phone', error: !!err.phone }">
				<text class="slot-tag">TEL</text>
				<view class="slot-main">
					<text class="slot-label">手机号</text>
					<input class="input input-num" v-model="phone" type="number" :placeholder-style="phStyle" placeholder="11 位手机号" maxlength="11"
						@focus="focus = 'phone'" @blur="focus = ''" />
				</view>
			</view>
			<text v-if="err.phone" class="err">{{ err.phone }}</text>

			<view class="slot" :class="{ active: focus === 'code', error: !!err.code }">
				<text class="slot-tag">OTP</text>
				<view class="slot-main">
					<text class="slot-label">波段</text>
					<view class="code-cell">
						<text class="code" :class="{ off: countdown > 0 }" @click="sendCode">{{ countdown > 0 ? countdown + ' s' : 'SEND' }}</text>
						<view class="code-bar" :style="{ width: (countdown > 0 ? countdown / 60 * 100 : 100) + '%' }"></view>
					</view>
				</view>
			</view>
			<text v-if="err.code" class="err">{{ err.code }}</text>

			<!-- 六格分离验证码 -->
			<view class="otp-wrap">
				<view class="otp-row">
					<view v-for="i in 6" :key="i" class="otp-cell"
						:class="{ fill: code.length >= i, cursor: focus === 'code' && code.length === i - 1 }">
						{{ code[i - 1] || '' }}
					</view>
				</view>
				<input class="otp-input" v-model="code" type="number" maxlength="6"
					@focus="focus = 'code'" @blur="focus = ''" />
			</view>

			<view class="btn" :class="{ charging: submitting }" @click="submit">
				{{ submitting ? 'CHARGING…' : '能量充压 · 验证放行' }}
			</view>

			<view class="check" @click="agreed = !agreed">
				<view class="check-box" :class="{ on: agreed }"><view class="check-tick"></view></view>
				<text class="check-txt">我已阅读并同意 <text class="link">《用户协议》</text> 与 <text class="link">《隐私政策》</text></text>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text>其他通道</text>
				<text class="link" @click="goSeg(2)">邮箱验证</text>
				<text class="link" @click="goSeg(0)">口令登录</text>
			</view>
			<text class="copy">© 2026 HOLO CORE · CLEARANCE L2</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				statusBar: 0,
				focus: '',
				agreed: false,
				submitting: false,
				phStyle: 'color:rgba(198,180,255,0.45);font-size:15px;',
				phone: '',
				code: '',
				err: {},
				countdown: 0,
				timer: null,
				orbSegs: ['密码', '短信', '邮箱', '注册', '重置'],
				orbIndex: 1
			}
		},
		onLoad() {
			const info = uni.getSystemInfoSync()
			this.statusBar = info.statusBarHeight || 0
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			toast(title) {
				uni.showToast({ title, icon: 'none' })
			},

			isPhone(v) {
				return /^1\d{10}$/.test(v)
			},

			goSeg(i) {
				if (i === this.orbIndex) return
				const paths = [
					'/pages/login129/login',
					'/pages/login129/phone-login',
					'/pages/login129/email-login',
					'/pages/login129/register',
					'/pages/login129/forget'
				]
				uni.redirectTo({ url: paths[i] })
			},

			sendCode() {
				if (this.countdown > 0) return
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
				this.countdown = 60
				// TODO: 调用后端发送短信验证码接口
				this.toast('波段已发出')
				this.startTimer()
			},

			startTimer() {
				if (this.timer) return
				this.timer = setInterval(() => {
					this.countdown--
					if (this.countdown <= 0) this.clearTimer()
				}, 1000)
			},

			clearTimer() {
				if (this.timer) {
					clearInterval(this.timer)
					this.timer = null
				}
			},

			submit() {
				if (this.submitting) return
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				const e = {}
				if (!this.isPhone(this.phone)) e.phone = '手机号格式不正确'
				if (!this.code || this.code.length < 6) e.code = '请输入 6 位验证码'
				this.err = e
				if (Object.keys(e).length > 0) return
				this.submitting = true
				// TODO: 调用后端验证码登录接口，成功后跳转首页
				setTimeout(() => {
					this.submitting = false
					this.toast('波段对准 · 放行')
				}, 800)
			},

			goBack() {
				const pages = getCurrentPages()
				if (pages.length > 1) uni.navigateBack()
				else uni.reLaunch({ url: '/pages/index' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
