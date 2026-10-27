<template>
	<view class="page">
		<view class="status" :style="{ height: statusBar + 'px' }"></view>

		<view class="nav">
			<view class="back" @click="goBack"><view class="back-line"></view></view>
			<text class="nav-idx">CORE 05 / 05</text>
		</view>

		<view class="core-head">
			<view class="crest"></view>
			<view class="core-txt">
				<text class="core-title">HOLO CORE // 全息中枢</text>
				<text class="core-sub">KEY RESET · RECOVERY</text>
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
			<text class="title">重铸口令</text>
			<text class="lead">核对身份波段后设定新口令，能量达到 MAX 才可铸造。</text>
		</view>

		<view class="panel">
			<view class="slot" :class="{ active: focus === 'account', error: !!err.account }">
				<text class="slot-tag">ID</text>
				<view class="slot-main">
					<text class="slot-label">身份</text>
					<input class="input" v-model="account" :placeholder-style="phStyle" placeholder="注册时的手机号 / 邮箱" maxlength="40"
						@focus="focus = 'account'" @blur="focus = ''" />
				</view>
			</view>
			<text v-if="err.account" class="err">{{ err.account }}</text>

			<view class="slot" :class="{ active: focus === 'code', error: !!err.code }">
				<text class="slot-tag">OTP</text>
				<view class="slot-main">
					<text class="slot-label">波段</text>
					<input class="input input-num" v-model="code" type="number" :placeholder-style="phStyle" placeholder="6 位验证码" maxlength="6"
						@focus="focus = 'code'" @blur="focus = ''" />
					<view class="code-cell">
						<text class="code" :class="{ off: countdown > 0 }" @click="sendCode">{{ countdown > 0 ? countdown + ' s' : 'SEND' }}</text>
						<view class="code-bar" :style="{ width: (countdown > 0 ? countdown / 60 * 100 : 100) + '%' }"></view>
					</view>
				</view>
			</view>
			<text v-if="err.code" class="err">{{ err.code }}</text>

			<view class="slot" :class="{ active: focus === 'password', error: !!err.password }">
				<text class="slot-tag">KEY</text>
				<view class="slot-main">
					<text class="slot-label">新口令</text>
					<input class="input" v-model="password" :password="!showPwd" :placeholder-style="phStyle" placeholder="8-20 位新口令" maxlength="20"
						@focus="focus = 'password'" @blur="focus = ''" />
					<text class="eye" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
				</view>
			</view>
			<text v-if="err.password" class="err">{{ err.password }}</text>

			<!-- 能量强度条 -->
			<view class="pwr">
				<view v-for="i in 4" :key="i" class="pwr-bar" :class="{ on: pwrLevel >= i, hot: pwrLevel >= 3 && pwrLevel >= i }"></view>
				<text class="pwr-txt">ENERGY {{ ['EMPTY', 'LOW', 'MID', 'HIGH', 'MAX'][pwrLevel] }}</text>
			</view>

			<view class="btn" :class="{ charging: submitting }" @click="submit">
				{{ submitting ? 'CHARGING…' : '能量充压 · 重铸口令' }}
			</view>

			<view class="check" @click="agreed = !agreed">
				<view class="check-box" :class="{ on: agreed }"><view class="check-tick"></view></view>
				<text class="check-txt">我已阅读并同意 <text class="link">《用户协议》</text> 与 <text class="link">《隐私政策》</text></text>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text>想起口令</text>
				<text class="link" @click="goSeg(0)">返回认证</text>
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
				showPwd: false,
				agreed: false,
				submitting: false,
				phStyle: 'color:rgba(198,180,255,0.45);font-size:15px;',
				account: '',
				code: '',
				password: '',
				err: {},
				countdown: 0,
				timer: null,
				orbSegs: ['密码', '短信', '邮箱', '注册', '重置'],
				orbIndex: 4
			}
		},
		computed: {
			pwrLevel() {
				const v = this.password || ''
				if (!v) return 0
				let lv = 0
				if (v.length >= 6) lv++
				if (v.length >= 10) lv++
				if (/[A-Za-z]/.test(v) && /\d/.test(v)) lv++
				if (v.length >= 12 && /[^A-Za-z0-9]/.test(v)) lv++
				return lv
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

			isValidAccount(v) {
				if (!v) return false
				if (v.indexOf('@') > -1) return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v)
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
				if (!this.account) return this.toast('请先输入身份代码')
				this.countdown = 60
				// TODO: 调用后端发送验证码接口（按身份类型走短信/邮件）
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
				if (!this.isValidAccount(this.account)) e.account = '请输入注册时的手机号 / 邮箱'
				if (!this.code || this.code.length < 6) e.code = '请输入 6 位验证码'
				if (!this.password || this.password.length < 8) e.password = '新口令不少于 8 位'
				this.err = e
				if (Object.keys(e).length > 0) return
				this.submitting = true
				// TODO: 调用后端重置密码接口，成功后返回登录页
				setTimeout(() => {
					this.submitting = false
					this.toast('口令重铸完成')
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
