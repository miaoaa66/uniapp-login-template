<template>
	<view class="page">
		<text class="watermark">127</text>

		<view class="extra">
			<view class="extra-stamp">季</view>
			<view class="extra-txt">
				<text class="extra-cn">THE EXTRA · 第 127 期 · 号外</text>
				<text class="extra-en">Extra Issue, Ink &amp; Print</text>
			</view>
		</view>

		<view class="head">
			<view class="title">纸上重逢</view>
			<view class="deck">在署名处落笔，在印鉴处盖章。油墨未干之前，这一页仍然属于你。</view>
		</view>

		<view class="bookmarks">
			<view v-for="(item, i) in bookmarks" :key="i" class="bm" :class="{ on: i === current }" @click="go(item)">{{ item.name }}</view>
		</view>

		<view class="form">
			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'account' }">
					<view class="field-line">
						<text class="field-name">署名：</text>
						<text class="field-en">ACCOUNT</text>
						<input class="input" v-model="account" placeholder="手机号 / 邮箱 / 工号" placeholder-class="ph" maxlength="40"
							@focus="focusField = 'account'" @blur="focusField = ''" />
					</view>
				</view>
			</view>

			<view class="field">
				<view class="field-box" :class="{ active: focusField === 'password' }">
					<view class="field-line">
						<text class="field-name">口令：</text>
						<text class="field-en">PASSWORD</text>
						<input class="input" v-model="password" :password="!showPassword" placeholder="请输入登录口令" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password'" @blur="focusField = ''" />
						<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐去' : '显露' }}</text>
					</view>
				</view>
			</view>

			<view class="row-between">
				<view class="remember" @click="remember = !remember">
					<view class="checkbox" :class="{ on: remember }"></view>
					<text>记住此设备</text>
				</view>
				<text class="link" @click="go(bookmarks[4])">口令遗失？登报挂失</text>
			</view>

			<view class="stamp-wrap">
				<view class="stamp-btn" :class="{ stamping: stamping }" @click="submit">
					<text class="stamp-char">盖印</text>
				</view>
				<text class="stamp-legend">STAMP TO SIGN IN</text>
			</view>

			<view class="foot-link">
				尚未登记？<text class="link" @click="go(bookmarks[3])">填写读者登记表</text>
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
				current: 0,
				account: '',
				password: '',
				showPassword: false,
				remember: true,
				agreed: false,
				focusField: '',
				stamping: false
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
				if (item.path === '/pages/login127/login') return
				uni.redirectTo({
					url: item.path
				})
			},

			submit() {
				if (this.stamping) return
				if (!this.agreed) return this.toast('请先阅读并同意读者协议')
				if (!this.account) return this.toast('请在署名处落笔')
				if (!this.password) return this.toast('请输入登录口令')
				this.stamping = true
				// TODO: 调用后端登录接口，成功后跳转首页
				setTimeout(() => {
					this.stamping = false
					this.toast('盖章生效 · 登录成功')
				}, 600)
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
