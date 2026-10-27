<template>
	<view class="page">
		<view class="status" :style="{ height: statusBar + 'px' }"></view>

		<view class="nav">
			<view class="back" @click="goBack"><view class="back-line"></view></view>
			<text class="nav-idx">CORE 04 / 05</text>
		</view>

		<view class="core-head">
			<view class="crest"></view>
			<view class="core-txt">
				<text class="core-title">HOLO CORE // 全息中枢</text>
				<text class="core-sub">NEW CREW · IDENTITY BUILD</text>
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
			<text class="title">登记新乘员</text>
			<text class="lead">建立身份档案，能量强度达到 MAX 后即可铸造口令。</text>
		</view>

		<view class="panel">
			<view class="slot" :class="{ active: focus === 'nickname', error: !!err.nickname }">
				<text class="slot-tag">CALL</text>
				<view class="slot-main">
					<text class="slot-label">呼号</text>
					<input class="input" v-model="nickname" :placeholder-style="phStyle" placeholder="你的乘员呼号" maxlength="20"
						@focus="focus = 'nickname'" @blur="focus = ''" />
				</view>
			</view>
			<text v-if="err.nickname" class="err">{{ err.nickname }}</text>

			<view class="slot" :class="{ active: focus === 'account', error: !!err.account }">
				<text class="slot-tag">ID</text>
				<view class="slot-main">
					<text class="slot-label">身份</text>
					<input class="input" v-model="account" :placeholder-style="phStyle" placeholder="手机号 / 邮箱" maxlength="40"
						@focus="focus = 'account'" @blur="focus = ''" />
				</view>
			</view>
			<text v-if="err.account" class="err">{{ err.account }}</text>

			<view class="slot" :class="{ active: focus === 'password', error: !!err.password }">
				<text class="slot-tag">KEY</text>
				<view class="slot-main">
					<text class="slot-label">口令</text>
					<input class="input" v-model="password" :password="!showPwd" :placeholder-style="phStyle" placeholder="8-20 位，建议字母加数字" maxlength="20"
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

			<view class="slot" :class="{ active: focus === 'password2', error: !!err.password2 }">
				<text class="slot-tag">KEY2</text>
				<view class="slot-main">
					<text class="slot-label">复核</text>
					<input class="input" v-model="password2" :password="!showPwd" :placeholder-style="phStyle" placeholder="请再次输入口令" maxlength="20"
						@focus="focus = 'password2'" @blur="focus = ''" />
				</view>
			</view>
			<text v-if="err.password2" class="err">{{ err.password2 }}</text>

			<view class="btn" :class="{ charging: submitting }" @click="submit">
				{{ submitting ? 'CHARGING…' : '铸造身份 · 完成登记' }}
			</view>

			<view class="check" @click="agreed = !agreed">
				<view class="check-box" :class="{ on: agreed }"><view class="check-tick"></view></view>
				<text class="check-txt">我已阅读并同意 <text class="link">《用户协议》</text> 与 <text class="link">《隐私政策》</text></text>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text>已有档案</text>
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
				nickname: '',
				account: '',
				password: '',
				password2: '',
				err: {},
				orbSegs: ['密码', '短信', '邮箱', '注册', '重置'],
				orbIndex: 3
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
				if (!this.nickname) e.nickname = '请输入乘员呼号'
				if (!this.isValidAccount(this.account)) e.account = '身份代码格式不正确（手机号 / 邮箱）'
				if (!this.password || this.password.length < 8) e.password = '口令不少于 8 位'
				if (this.password !== this.password2) e.password2 = '两次口令不一致'
				this.err = e
				return Object.keys(e).length === 0
			},

			submit() {
				if (this.submitting) return
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				if (!this.validate()) return
				this.submitting = true
				// TODO: 调用后端注册接口，成功后返回登录页
				setTimeout(() => {
					this.submitting = false
					this.toast('身份铸造完成')
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
