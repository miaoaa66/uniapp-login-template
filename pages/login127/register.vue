<template>
	<view class="page">
		<text class="watermark">127</text>

		<view class="extra">
			<view class="extra-stamp">季</view>
			<view class="extra-txt">
				<text class="extra-cn">THE EXTRA · 第 127 期 · 读者登记表</text>
				<text class="extra-en">Reader Registration Form</text>
			</view>
		</view>

		<view class="head">
			<view class="title">填一张登记表</view>
			<view class="deck">像登记一张旧式借书卡：逐栏填写，交由编辑部核对，盖章后正式入册。</view>
		</view>

		<view class="bookmarks">
			<view v-for="(item, i) in bookmarks" :key="i" class="bm" :class="{ on: i === current }" @click="go(item)">{{ item.name }}</view>
		</view>

		<view class="form">
			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'nickname' }">
					<view class="field-line">
						<text class="field-name">笔名：</text>
						<input class="input" v-model="nickname" placeholder="将印在读者名册上" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'nickname'" @blur="focusField = ''" />
					</view>
				</view>
				<text v-if="err.nickname" class="err">{{ err.nickname }}</text>
			</view>

			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'phone' }">
					<view class="field-line">
						<text class="field-name">手机号：</text>
						<input class="input" v-model="phone" type="number" placeholder="11 位手机号" placeholder-class="ph" maxlength="11"
							@focus="focusField = 'phone'" @blur="focusField = ''" />
					</view>
				</view>
				<text v-if="err.phone" class="err">{{ err.phone }}</text>
			</view>

			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'email' }">
					<view class="field-line">
						<text class="field-name">邮箱：</text>
						<input class="input" v-model="email" placeholder="用于接收样刊通知" placeholder-class="ph" maxlength="60"
							@focus="focusField = 'email'" @blur="focusField = ''" />
					</view>
				</view>
				<text v-if="err.email" class="err">{{ err.email }}</text>
			</view>

			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'password' }">
					<view class="field-line">
						<text class="field-name">口令：</text>
						<input class="input" v-model="password" :password="!showPassword" placeholder="8-20 位，建议字母加数字" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password'" @blur="focusField = ''" />
						<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐去' : '显露' }}</text>
					</view>
				</view>
				<text v-if="err.password" class="err">{{ err.password }}</text>
			</view>

			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'password2' }">
					<view class="field-line">
						<text class="field-name">复核口令：</text>
						<input class="input" v-model="password2" :password="true" placeholder="请再次输入口令" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password2'" @blur="focusField = ''" />
					</view>
				</view>
				<text v-if="err.password2" class="err">{{ err.password2 }}</text>
			</view>

			<view class="stamp-wrap">
				<view class="stamp-btn" :class="{ stamping: stamping }" @click="submit">
					<text class="stamp-char">入册</text>
				</view>
				<text class="stamp-legend">STAMP TO REGISTER</text>
			</view>

			<view class="foot-link">
				已在册？<text class="link" @click="go(bookmarks[0])">回到密码页盖章</text>
			</view>

			<view class="agreement">
				<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
				<text>本人已阅读并同意<text class="link">《读者协议》</text>与<text class="link">《信息保密条款》</text>，盖章即视为签署。</text>
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
				current: 3,
				nickname: '',
				phone: '',
				email: '',
				password: '',
				password2: '',
				showPassword: false,
				agreed: false,
				focusField: '',
				stamping: false,
				err: {}
			}
		},
		methods: {
			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			go(item) {
				if (item.path === '/pages/login127/register') return
				uni.redirectTo({
					url: item.path
				})
			},

			validate() {
				const e = {}
				if (!this.nickname) e.nickname = 'No.01 尚未填写笔名'
				if (!/^1\d{10}$/.test(this.phone)) e.phone = 'No.02 手机号格式不正确'
				if (!/^[\w.-]+@[\w-]+\.[\w.-]+$/.test(this.email)) e.email = 'No.03 邮箱格式不正确'
				if (!this.password || this.password.length < 8) e.password = 'No.04 口令不少于 8 位'
				if (this.password !== this.password2) e.password2 = 'No.05 两次口令不一致'
				this.err = e
				return Object.keys(e).length === 0
			},

			submit() {
				if (this.stamping) return
				if (!this.agreed) return this.toast('请先阅读并同意读者协议')
				if (!this.validate()) return
				this.stamping = true
				// TODO: 调用后端注册接口，成功后返回登录页
				setTimeout(() => {
					this.stamping = false
					this.toast('盖章入册 · 注册成功')
				}, 600)
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
