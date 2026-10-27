<template>
	<view class="page">
		<text class="watermark">127</text>

		<view class="extra">
			<view class="extra-stamp">季</view>
			<view class="extra-txt">
				<text class="extra-cn">THE EXTRA · 第 127 期 · 电讯附刊</text>
				<text class="extra-en">Telegraph Annex, SMS</text>
			</view>
		</view>

		<view class="head">
			<view class="title">凭电讯登岸</view>
			<view class="deck">一串电码即可核明身份。未登记的号码，将自动补入本期订户名录。</view>
		</view>

		<view class="bookmarks">
			<view v-for="(item, i) in bookmarks" :key="i" class="bm" :class="{ on: i === current }" @click="go(item)">{{ item.name }}</view>
		</view>

		<view class="form">
			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'phone' }">
					<view class="field-line">
						<text class="field-name">号码：</text>
						<text class="field-prefix">+86</text>
						<input class="input" v-model="phone" type="number" placeholder="请输入手机号" placeholder-class="ph" maxlength="11"
							@focus="focusField = 'phone'" @blur="focusField = ''" />
					</view>
				</view>
				<text v-if="err.phone" class="err">{{ err.phone }}</text>
			</view>

			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'code' }">
					<view class="field-line">
						<text class="field-name">电码：</text>
						<text class="field-en">CODE</text>
						<input class="input" v-model="code" type="number" placeholder="6 位验证码" placeholder-class="ph" maxlength="6"
							@focus="focusField = 'code'" @blur="focusField = ''" />
						<text class="code-btn" :class="{ dim: countdown > 0 }" @click="sendCode">
							{{ countdown > 0 ? countdown + 's' : '拍发电码' }}
						</text>
					</view>
				</view>
				<text v-if="err.code" class="err">{{ err.code }}</text>
			</view>

			<view class="stamp-wrap">
				<view class="stamp-btn" :class="{ stamping: stamping }" @click="submit">
					<text class="stamp-char">核码</text>
				</view>
				<text class="stamp-legend">STAMP TO VERIFY</text>
			</view>

			<view class="foot-link">
				或改用<text class="link" @click="go(bookmarks[2])">邮路（邮箱验证码）</text>登岸
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
				current: 1,
				phone: '',
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

			isPhone(v) {
				return /^1\d{10}$/.test(v)
			},

			go(item) {
				if (item.path === '/pages/login127/phone-login') return
				uni.redirectTo({
					url: item.path
				})
			},

			sendCode() {
				if (this.countdown > 0) return
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
				this.countdown = 60
				// TODO: 调用后端发送短信验证码接口
				this.toast('电码已拍发')
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
				if (!this.isPhone(this.phone)) e.phone = '手机号格式不正确'
				if (!this.code) e.code = '请输入电码'
				this.err = e
				if (Object.keys(e).length > 0) return
				this.stamping = true
				// TODO: 调用后端验证码登录接口，成功后跳转首页
				setTimeout(() => {
					this.stamping = false
					this.toast('核码通过 · 登录成功')
				}, 600)
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
