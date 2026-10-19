<template>
	<view class="page">
		<view class="paper">
			<view class="masthead-meta">
				<text>第 107 期 · 公告</text>
				<text>2026 年 10 月 19 日</text>
				<text>遗失声明</text>
			</view>
			<view class="rule-double"></view>
			<view class="brand-line">
				<text class="brand">晨报</text>
				<text class="brand-en">THE MORNING POST</text>
			</view>
			<view class="rule-thin"></view>

			<view class="kicker">公告 · 口令重设</view>
			<view class="headline">遗失口令声明</view>
			<view class="deck">若你遗忘了登录口令，请在此登报声明。核对投递地址后，编辑部将协助你重新签发新口令。</view>

			<view class="form">
				<view class="field">
					<text class="label">投递地址 / CONTACT</text>
					<view class="field-line">
						<input class="input" v-model="contact" placeholder="手机号 / 邮箱" placeholder-class="ph" maxlength="40" />
					</view>
				</view>

				<view class="field">
					<text class="label">验证码 / CODE</text>
					<view class="field-line">
						<input class="input" v-model="code" placeholder="请输入 6 位验证码" placeholder-class="ph" maxlength="6" />
						<text class="code-btn" :class="{ disabled: counting }" @click="sendCode">{{ counting ? seconds + ' 秒后重发' : '获取验证码' }}</text>
					</view>
				</view>

				<view class="field">
					<text class="label">新口令 / NEW PASSWORD</text>
					<view class="field-line">
						<input class="input" v-model="password" :password="!showPassword" placeholder="8-20 位，建议含字母与数字" placeholder-class="ph" maxlength="20" />
						<text class="toggle" @click="showPassword = !showPassword">{{ showPassword ? '隐去' : '显露' }}</text>
					</view>
				</view>

				<view class="field">
					<text class="label">确认新口令 / CONFIRM</text>
					<view class="field-line">
						<input class="input" v-model="password2" :password="!showPassword" placeholder="请再次输入新口令" placeholder-class="ph" maxlength="20" />
					</view>
				</view>

				<view class="submit" @click="submit">
					<text class="submit-text">重 设 口 令</text>
					<view class="stamp">登报</view>
				</view>

				<view class="foot-link">
					想起口令了？<text class="link" @click="goLogin">返回签署</text>
				</view>
			</view>

			<view class="colophon">
				<text>A3 版 · 遗失声明</text>
				<text>本报编辑部</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				contact: '',
				code: '',
				password: '',
				password2: '',
				showPassword: false,
				counting: false,
				seconds: 60,
				timer: null
			}
		},
		onUnload() {
			if (this.timer) clearInterval(this.timer)
		},
		methods: {
			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			sendCode() {
				if (this.counting) return
				if (!this.contact) return this.toast('请先填写投递地址')
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发出')
				this.counting = true
				this.seconds = 60
				this.timer = setInterval(() => {
					this.seconds--
					if (this.seconds <= 0) {
						clearInterval(this.timer)
						this.counting = false
					}
				}, 1000)
			},

			submit() {
				if (!this.contact) return this.toast('请输入投递地址')
				if (!this.code) return this.toast('请输入验证码')
				if (!this.password) return this.toast('请输入新口令')
				if (this.password !== this.password2) return this.toast('两次输入的口令不一致')
				// TODO: 调用后端重置密码接口，成功后返回登录页
				this.toast('口令已重设')
			},

			goLogin() {
				uni.navigateBack()
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
