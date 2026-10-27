<template>
	<view class="page">
		<text class="watermark">127</text>

		<view class="extra">
			<view class="extra-stamp">季</view>
			<view class="extra-txt">
				<text class="extra-cn">THE EXTRA · 第 127 期 · 遗失声明</text>
				<text class="extra-en">Lost &amp; Found Notice</text>
			</view>
		</view>

		<view class="head">
			<view class="title">登报挂失</view>
			<view class="deck">口令遗失不必惊慌。留下通联方式，核对无误后即可重新镌刻一枚新印。</view>
		</view>

		<view class="bookmarks">
			<view v-for="(item, i) in bookmarks" :key="i" class="bm" :class="{ on: i === current }" @click="go(item)">{{ item.name }}</view>
		</view>

		<view class="form">
			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'account' }">
					<view class="field-line">
						<text class="field-name">通联：</text>
						<text class="field-en">CONTACT</text>
						<input class="input" v-model="account" placeholder="注册时的手机号 / 邮箱" placeholder-class="ph" maxlength="40"
							@focus="focusField = 'account'" @blur="focusField = ''" />
					</view>
				</view>
				<text v-if="err.account" class="err">{{ err.account }}</text>
			</view>

			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'code' }">
					<view class="field-line">
						<text class="field-name">凭证：</text>
						<text class="field-en">CODE</text>
						<input class="input" v-model="code" type="number" placeholder="6 位验证码" placeholder-class="ph" maxlength="6"
							@focus="focusField = 'code'" @blur="focusField = ''" />
						<text class="code-btn" :class="{ dim: countdown > 0 }" @click="sendCode">
							{{ countdown > 0 ? countdown + 's' : '寄送凭证' }}
						</text>
					</view>
				</view>
				<text v-if="err.code" class="err">{{ err.code }}</text>
			</view>

			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'password' }">
					<view class="field-line">
						<text class="field-name">新印：</text>
						<text class="field-en">NEW KEY</text>
						<input class="input" v-model="password" :password="!showPassword" placeholder="8-20 位新口令" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password'" @blur="focusField = ''" />
						<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐去' : '显露' }}</text>
					</view>
				</view>
				<text v-if="err.password" class="err">{{ err.password }}</text>
			</view>

			<view class="stamp-wrap">
				<view class="stamp-btn" :class="{ stamping: stamping }" @click="submit">
					<text class="stamp-char">重镌</text>
				</view>
				<text class="stamp-legend">STAMP TO RESET</text>
			</view>

			<view class="foot-link">
				想起口令了？<text class="link" @click="go(bookmarks[0])">撕掉声明，回去登录</text>
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
				current: 4,
				account: '',
				code: '',
				password: '',
				showPassword: false,
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

			go(item) {
				if (item.path === '/pages/login127/forget') return
				uni.redirectTo({
					url: item.path
				})
			},

			sendCode() {
				if (this.countdown > 0) return
				if (!this.account) return this.toast('请先填写通联方式')
				this.countdown = 60
				// TODO: 调用后端发送验证码接口（按通联类型走短信/邮件）
				this.toast('凭证已寄出')
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

			validate() {
				const e = {}
				if (!this.account) e.account = '请填写通联方式'
				if (!this.code) e.code = '请输入验证码'
				if (!this.password || this.password.length < 8) e.password = '新口令不少于 8 位'
				this.err = e
				return Object.keys(e).length === 0
			},

			submit() {
				if (this.stamping) return
				if (!this.agreed) return this.toast('请先阅读并同意读者协议')
				if (!this.validate()) return
				this.stamping = true
				// TODO: 调用后端重置密码接口，成功后返回登录页
				setTimeout(() => {
					this.stamping = false
					this.toast('新印镌成 · 请重新登录')
				}, 600)
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
