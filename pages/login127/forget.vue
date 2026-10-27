<template>
	<view class="page">
		<text class="spine">纸页之间 · 文学季刊</text>

		<view class="masthead">
			<view class="rule-double"></view>
			<view class="kicker">
				<text class="kicker-cn">VOL.127 · 遗失声明</text>
				<text class="kicker-en">LOST PASSWORD</text>
			</view>
			<view class="rule-thin"></view>
		</view>

		<view class="head">
			<view class="title">寻回你的口令</view>
			<view class="deck">别担心。核对通联信息后，即可重新设定口令，续上未完的篇章。</view>
		</view>

		<view class="toc">
			<view v-for="(item, i) in chapters" :key="i" class="toc-item" :class="{ on: i === current }" @click="goChapter(item)">
				<text class="toc-idx">{{ item.idx }}</text>{{ item.name }}
			</view>
		</view>

		<view class="form">
			<view class="field">
				<view class="label">
					<text>通联</text>
					<text class="label-en">CONTACT</text>
				</view>
				<view class="field-box" :class="{ active: focusField === 'account' }">
					<view class="field-line">
						<input class="input" v-model="account" placeholder="请输入登记时的手机号 / 邮箱" placeholder-class="ph" maxlength="40"
							@focus="focusField = 'account'" @blur="focusField = ''" />
					</view>
				</view>
			</view>

			<view class="field">
				<view class="label">
					<text>验证码</text>
					<text class="label-en">CODE</text>
				</view>
				<view class="field-box" :class="{ active: focusField === 'code' }">
					<view class="field-line">
						<input class="input" v-model="code" type="number" placeholder="请输入验证码" placeholder-class="ph" maxlength="6"
							@focus="focusField = 'code'" @blur="focusField = ''" />
						<text class="code-btn" :class="{ dim: countdown > 0 }" @click="sendCode">
							{{ countdown > 0 ? countdown + 's' : '获取验证码' }}
						</text>
					</view>
				</view>
			</view>

			<view class="field">
				<view class="label">
					<text>新口令</text>
					<text class="label-en">NEW PASSWORD</text>
				</view>
				<view class="field-box" :class="{ active: focusField === 'password' }">
					<view class="field-line">
						<input class="input" v-model="password" :password="!showPassword" placeholder="8-20 位，建议字母加数字" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password'" @blur="focusField = ''" />
						<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐去' : '显露' }}</text>
					</view>
				</view>
			</view>

			<view class="submit" @click="submit">
				<text class="submit-text">重 设 口 令</text>
			</view>

			<view class="foot-link">
				想起口令了？<text class="link" @click="goChapter(chapters[0])">返回登录</text>
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
				current: 4,
				account: '',
				code: '',
				password: '',
				showPassword: false,
				agreed: false,
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

			goChapter(item) {
				if (item.idx === '05') return
				uni.redirectTo({
					url: item.path
				})
			},

			sendCode() {
				if (this.countdown > 0) return
				if (!this.account) return this.toast('请输入手机号或邮箱')
				this.countdown = 60
				// TODO: 调用后端发送验证码接口
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
				if (!this.account) return this.toast('请输入手机号或邮箱')
				if (!this.code) return this.toast('请输入验证码')
				if (!this.password) return this.toast('请输入新口令')
				if (!this.agreed) return this.toast('请先阅读并同意读者协议')
				// TODO: 调用后端重置密码接口，成功后返回登录页
				this.toast('口令已重设')
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
