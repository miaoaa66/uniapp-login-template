<template>
	<view class="page">
		<text class="spine">纸页之间 · 文学季刊</text>

		<view class="masthead">
			<view class="rule-double"></view>
			<view class="kicker">
				<text class="kicker-cn">VOL.127 · 短信凭证</text>
				<text class="kicker-en">SMS ACCESS</text>
			</view>
			<view class="rule-thin"></view>
		</view>

		<view class="head">
			<view class="title">以短信为凭证</view>
			<view class="deck">未登记的手机号，将自动为你开立一份读者档案。</view>
		</view>

		<view class="toc">
			<view v-for="(item, i) in chapters" :key="i" class="toc-item" :class="{ on: i === current }" @click="goChapter(item)">
				<text class="toc-idx">{{ item.idx }}</text>{{ item.name }}
			</view>
		</view>

		<view class="form">
			<view class="field">
				<view class="label">
					<text>手机号</text>
					<text class="label-en">PHONE</text>
				</view>
				<view class="field-box" :class="{ active: focusField === 'phone' }">
					<view class="field-line">
						<text class="field-prefix">+86</text>
						<input class="input" v-model="phone" type="number" placeholder="请输入手机号" placeholder-class="ph" maxlength="11"
							@focus="focusField = 'phone'" @blur="focusField = ''" />
					</view>
				</view>
			</view>

			<view class="field">
				<view class="label">
					<text>短信验证码</text>
					<text class="label-en">CODE</text>
				</view>
				<view class="field-box" :class="{ active: focusField === 'code' }">
					<view class="field-line">
						<input class="input" v-model="code" type="number" placeholder="请输入 6 位验证码" placeholder-class="ph" maxlength="6"
							@focus="focusField = 'code'" @blur="focusField = ''" />
						<text class="code-btn" :class="{ dim: countdown > 0 }" @click="sendCode">
							{{ countdown > 0 ? countdown + 's' : '获取验证码' }}
						</text>
					</view>
				</view>
			</view>

			<view class="submit" @click="submit">
				<text class="submit-text">验 证 登 录</text>
			</view>

			<view class="foot-link">
				或使用<text class="link" @click="goChapter(chapters[2])">邮箱验证码</text>登录
			</view>

			<view class="agreement">
				<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
				<text>本人已阅读并同意<text class="link">《读者协议》</text>与<text class="link">《信息保密条款》</text></text>
			</view>

			<view class="foot-note">THE QUARTERLY · 文学季刊编辑部</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				chapters: [
					{ idx: '01', name: '密码登录', path: '/pages/login127/login' },
					{ idx: '02', name: '手机验证码', path: '/pages/login127/phone-login' },
					{ idx: '03', name: '邮箱验证码', path: '/pages/login127/email-login' },
					{ idx: '04', name: '注册账号', path: '/pages/login127/register' },
					{ idx: '05', name: '找回密码', path: '/pages/login127/forget' }
				],
				current: 1,
				agreed: false,
				phone: '',
				code: '',
				focusField: '',
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

			isPhone(value) {
				return /^1\d{10}$/.test(value)
			},

			goChapter(item) {
				if (item.idx === '02') return
				uni.redirectTo({
					url: item.path
				})
			},

			sendCode() {
				if (this.countdown > 0) return
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
				this.countdown = 60
				// TODO: 调用后端发送短信验证码接口
				this.toast('验证码已发送')
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
				if (!this.agreed) return this.toast('请先阅读并同意读者协议')
				if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
				if (!this.code) return this.toast('请输入短信验证码')
				// TODO: 调用后端验证码登录接口，成功后跳转首页
				this.toast('登录成功')
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
