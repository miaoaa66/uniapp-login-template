<template>
	<view class="page">
		<view class="status" :style="{ height: statusBar + 'px' }"></view>

		<view class="nav">
			<view class="back" @click="goBack"><view class="back-line"></view></view>
			<text class="nav-idx">CORE 01 / 05</text>
		</view>

		<view class="core-head">
			<view class="crest"></view>
			<view class="core-txt">
				<text class="core-title">HOLO CORE // 全息中枢</text>
				<text class="core-sub">AUTH TERMINAL · ACCESS L2</text>
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
			<text class="title">中枢口令认证</text>
			<text class="lead">输入身份代码与口令，能量充压后放行进入主舱。</text>
		</view>

		<view class="panel">
			<view class="slot" :class="{ active: focus === 'account', error: !!err.account }">
				<text class="slot-tag">ID</text>
				<view class="slot-main">
					<text class="slot-label">身份</text>
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

			<!-- 能量强度条 -->
			<view class="pwr">
				<view v-for="i in 4" :key="i" class="pwr-bar" :class="{ on: pwrLevel >= i, hot: pwrLevel >= 3 && pwrLevel >= i }"></view>
				<text class="pwr-txt">ENERGY {{ ['EMPTY', 'LOW', 'MID', 'HIGH', 'MAX'][pwrLevel] }}</text>
			</view>

			<view class="row">
				<view class="check" style="margin-top: 0;" @click="remember = !remember">
					<view class="check-box" :class="{ on: remember }"><view class="check-tick"></view></view>
					<text class="check-txt">记住此终端</text>
				</view>
				<text class="link link-soft" @click="goSeg(4)">口令遗失</text>
			</view>

			<view class="btn" :class="{ charging: submitting }" @click="submit">
				{{ submitting ? 'CHARGING…' : '能量充压 · 放行' }}
			</view>

			<view class="check" @click="agreed = !agreed">
				<view class="check-box" :class="{ on: agreed }"><view class="check-tick"></view></view>
				<text class="check-txt">我已阅读并同意 <text class="link">《用户协议》</text> 与 <text class="link">《隐私政策》</text></text>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text>其他通道</text>
				<text class="link" @click="goSeg(3)">登记新乘员</text>
				<text class="link" @click="goSeg(1)">短信验证</text>
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
				remember: false,
				agreed: false,
				submitting: false,
				phStyle: 'color:rgba(198,180,255,0.45);font-size:15px;',
				account: '',
				password: '',
				err: {},
				orbSegs: ['密码', '短信', '邮箱', '注册', '重置'],
				orbIndex: 0
			}
		},
		computed: {
			/* 能量强度：0-4 格，按长度与字符多样性 */
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

			validate() {
				const e = {}
				if (!this.account) e.account = '请输入身份代码'
				else if (!this.isValidAccount(this.account)) e.account = '身份代码格式不正确'
				if (!this.password) e.password = '请输入登录口令'
				else if (this.password.length < 6) e.password = '口令不少于 6 位'
				this.err = e
				return Object.keys(e).length === 0
			},

			submit() {
				if (this.submitting) return
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				if (!this.validate()) return
				this.submitting = true
				// TODO: 调用后端登录接口，成功后跳转首页
				setTimeout(() => {
					this.submitting = false
					this.toast('充压完成 · 放行')
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
