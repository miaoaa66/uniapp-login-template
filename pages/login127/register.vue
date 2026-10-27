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
			<view v-for="(f, i) in fieldDefs" :key="f.key" class="field">
				<view class="field-box" :class="{ active: focusField === f.key }">
					<view class="field-line">
						<text class="field-name">{{ f.no }} {{ f.name }}：</text>
						<input v-if="f.key !== 'password' && f.key !== 'password2'" class="input" v-model="form[f.key]"
							:type="f.type || 'text'" :placeholder="f.ph" placeholder-class="ph" :maxlength="f.max || 40"
							@focus="focusField = f.key" @blur="focusField = ''" />
						<input v-else class="input" v-model="form[f.key]" :password="!(f.key === 'password' && showPassword)"
							:placeholder="f.ph" placeholder-class="ph" maxlength="20"
							@focus="focusField = f.key" @blur="focusField = ''" />
						<text v-if="f.key === 'password'" class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐去' : '显露' }}</text>
					</view>
				</view>
				<text v-if="err[f.key]" class="err">{{ err[f.key] }}</text>
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
				fieldDefs: [
					{ key: 'nickname', no: 'No.01', name: '笔名', ph: '将印在读者名册上', max: 20 },
					{ key: 'phone', no: 'No.02', name: '手机号', ph: '11 位手机号', type: 'number', max: 11 },
					{ key: 'email', no: 'No.03', name: '邮箱', ph: '用于接收样刊通知', max: 60 },
					{ key: 'password', no: 'No.04', name: '口令', ph: '8-20 位，建议字母加数字' },
					{ key: 'password2', no: 'No.05', name: '复核口令', ph: '请再次输入口令' }
				],
				form: {
					nickname: '',
					phone: '',
					email: '',
					password: '',
					password2: ''
				},
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
				if (!this.form.nickname) e.nickname = 'No.01 尚未填写笔名'
				if (!/^1\d{10}$/.test(this.form.phone)) e.phone = 'No.02 手机号格式不正确'
				if (!/^[\w.-]+@[\w-]+\.[\w.-]+$/.test(this.form.email)) e.email = 'No.03 邮箱格式不正确'
				if (!this.form.password || this.form.password.length < 8) e.password = 'No.04 口令不少于 8 位'
				if (this.form.password !== this.form.password2) e.password2 = 'No.05 两次口令不一致'
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
