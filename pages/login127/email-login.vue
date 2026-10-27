<template>
	<view class="page">
		<text class="watermark">127</text>

		<view class="extra">
			<view class="extra-stamp">季</view>
			<view class="extra-txt">
				<text class="extra-cn">THE EXTRA · 第 127 期 · 邮路附刊</text>
				<text class="extra-en">Postal Annex, Mail</text>
			</view>
		</view>

		<view class="head">
			<view class="title">凭邮路登岸</view>
			<view class="deck">验证码将经由邮路寄达你的信箱，请留意查收与时效。</view>
		</view>

		<view class="bookmarks">
			<view v-for="(item, i) in bookmarks" :key="i" class="bm" :class="{ on: i === current }" @click="go(item)">{{ item.name }}</view>
		</view>

		<view class="form">
			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'email' }">
					<view class="field-line">
						<text class="field-name">信箱：</text>
						<text class="field-en">MAILBOX</text>
						<input class="input" v-model="email" placeholder="name@example.com" placeholder-class="ph" maxlength="60"
							@focus="focusField = 'email'" @blur="focusField = ''" />
					</view>
				</view>
				<text v-if="err.email" class="err">{{ err.email }}</text>
			</view>

			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'code' }">
					<view class="field-line">
						<text class="field-name">凭证：</text>
						<text class="field-en">CODE</text>
						<input class="input" v-model="code" type="number" placeholder="6 位验证码" placeholder-class="ph" maxlength="6"
							@focus="focusField = 'code'" @blur="focusField = ''" />
						<text class="code-btn" :class="{ dim: countdown > 0 }" @click="sendCode">
							{{ countdown > 0 ? countdown + 's' : '付邮寄出' }}
						</text>
					</view>
				</view>
				<text v-if="err.code" class="err">{{ err.code }}</text>
			</view>

			<view class="stamp-wrap">
				<view class="stamp-btn" :class="{ stamping: stamping }" @click="submit">
					<text class="stamp-char">核函</text>
				</view>
				<text class="stamp-legend">STAMP TO VERIFY</text>
			</view>

			<view class="foot-link">
				或改用<text class="link" @click="go(bookmarks[1])">电讯（短信验证码）</text>登岸
			</view>

			<view class="agreement">
				<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
				<text>本人已阅读并同意<text class="link">《读者协议》</text>与<text class="link">《信息保密条款》</text>。</text>
			</view>

			<view class="foot-note">THE EXTRA · 特刊编辑部承印</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				bookmarks: [
					{ name: '密码', path: '/pages/login127/login' },
					{ name: '短信', path: '/pages/login127/phone-login' },
					{ name: '邮箱', path: '/pages/login127/email-login' },
					{ name: '注册', path: '/pages/login127/register' },
					{ name: '挂失', path: '/pages/login127/forget' }
				],
				current: 2,
				email: '',
				code: '',
				agreed: false,
				focusField: '',
				stamping: false,
				err: {},
				countdown: 0,
				timer: null
			}
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			isEmail(v) {
				return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v)
			},

			go(item) {
				if (item.path === '/pages/login127/email-login') return
				uni.redirectTo({
					url: item.path
				})
			},

			sendCode() {
				if (this.countdown > 0) return
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱地址')
				this.countdown = 60
				// TODO: 调用后端发送邮箱验证码接口
				this.toast('函件已付邮')
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
				if (this.stamping) return
				if (!this.agreed) return this.toast('请先阅读并同意读者协议')
				const e = {}
				if (!this.isEmail(this.email)) e.email = '邮箱格式不正确'
				if (!this.code) e.code = '请输入验证码'
				this.err = e
				if (Object.keys(e).length > 0) return
				this.stamping = true
				// TODO: 调用后端验证码登录接口，成功后跳转首页
				setTimeout(() => {
					this.stamping = false
					this.toast('核函通过 · 登录成功')
				}, 600)
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
