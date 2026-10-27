<template>
	<view class="page">
		<view class="status" :style="{ height: statusBar + 'px' }"></view>

		<view class="nav">
			<view class="back" @click="goBack"><view class="back-line"></view></view>
			<text class="nav-idx">SEC 02 / 05</text>
		</view>

		<view class="radar">
			<view class="radar-line-h"></view>
			<view class="radar-line-v"></view>
			<view class="radar-sweep"></view>
			<view class="radar-core">
				<text class="radar-v">NEW</text>
				<text class="radar-h">NEW CREW</text>
			</view>
		</view>

		<view class="head">
			<text class="title">登记新乘员</text>
			<text class="lead">建立你的身份档案，随后即可进入主舱。</text>
		</view>

		<view class="slots">
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
				<text class="slot-tag">ACC</text>
				<view class="slot-main">
					<text class="slot-label">账号</text>
					<input class="input" v-model="account" :placeholder-style="phStyle" placeholder="手机号 / 邮箱" maxlength="40"
						@focus="focus = 'account'" @blur="focus = ''" />
				</view>
			</view>
			<text v-if="err.account" class="err">{{ err.account }}</text>

			<view class="slot" :class="{ active: focus === 'password', error: !!err.password }">
				<text class="slot-tag">KEY</text>
				<view class="slot-main">
					<text class="slot-label">口令</text>
					<input class="input" v-model="password" :password="!showPwd" :placeholder-style="phStyle" placeholder="8-20 位" maxlength="20"
						@focus="focus = 'password'" @blur="focus = ''" />
					<text class="eye" @click="showPwd = !showPwd">{{ showPwd ? '隐藏' : '显示' }}</text>
				</view>
			</view>
			<text v-if="err.password" class="err">{{ err.password }}</text>

			<view class="slot" :class="{ active: focus === 'password2', error: !!err.password2 }">
				<text class="slot-tag">KEY2</text>
				<view class="slot-main">
					<text class="slot-label">确认</text>
					<input class="input" v-model="password2" :password="!showPwd" :placeholder-style="phStyle" placeholder="再次输入口令" maxlength="20"
						@focus="focus = 'password2'" @blur="focus = ''" />
				</view>
			</view>
			<text v-if="err.password2" class="err">{{ err.password2 }}</text>

			<view class="btn" @click="submit">创建账户</view>

			<view class="check" style="margin-top: 24rpx;" @click="agreed = !agreed">
				<view class="check-box" :class="{ on: agreed }"><view class="check-tick"></view></view>
				<text class="check-txt">我已阅读并同意 <text class="link">《用户协议》</text> 与 <text class="link">《隐私政策》</text></text>
			</view>
		</view>

		<view class="foot">
			<view class="foot-row">
				<text>已是乘员</text>
				<text class="link" @click="goBack">返回登录</text>
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
				focus: '',
				showPwd: false,
				agreed: false,
				phStyle: 'color:rgba(94,140,136,0.8);font-size:15px;',
				nickname: '',
				account: '',
				password: '',
				password2: '',
				err: {}
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

			submit() {
				const e = {}
				if (!this.nickname) e.nickname = '请输入呼号'
				if (!this.account) e.account = '请输入账号'
				else if (!this.isValidAccount(this.account)) e.account = '账号格式不正确'
				if (!this.password) e.password = '请设置口令'
				else if (this.password.length < 6) e.password = '口令不少于 6 位'
				if (this.password2 !== this.password) e.password2 = '两次口令不一致'
				this.err = e
				if (Object.keys(e).length > 0) return
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端注册接口，成功后返回登录页
				this.toast('登记成功')
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
