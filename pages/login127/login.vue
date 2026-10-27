<template>
	<view class="page">
		<text class="spine">纸页之间 · 文学季刊</text>

		<view class="masthead">
			<view class="rule-double"></view>
			<view class="kicker">
				<text class="kicker-cn">VOL.127 · 登录特辑</text>
				<text class="kicker-en">SIGN-IN SPECIAL</text>
			</view>
			<view class="rule-thin"></view>
		</view>

		<view class="head">
			<view class="title">欢迎回到，纸页之间</view>
			<view class="deck">请以你的署名与口令，续写这一期的篇章。所有登录记录，均按档案规范归档留存。</view>
		</view>

		<view class="toc">
			<view v-for="(item, i) in chapters" :key="i" class="toc-item" :class="{ on: i === current }" @click="goChapter(item)">
				<text class="toc-idx">{{ item.idx }}</text>{{ item.name }}
			</view>
		</view>

		<view class="form">
			<view class="field">
				<view class="label">
					<text>署名</text>
					<text class="label-en">ACCOUNT</text>
				</view>
				<view class="field-box" :class="{ active: focusField === 'account' }">
					<view class="field-line">
						<input class="input" v-model="account" placeholder="手机号 / 邮箱 / 工号" placeholder-class="ph" maxlength="40"
							@focus="focusField = 'account'" @blur="focusField = ''" />
					</view>
				</view>
			</view>

			<view class="field">
				<view class="label">
					<text>口令</text>
					<text class="label-en">PASSWORD</text>
				</view>
				<view class="field-box" :class="{ active: focusField === 'password' }">
					<view class="field-line">
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
				<text class="link" @click="goChapter(chapters[4])">忘记口令？</text>
			</view>

			<view class="submit" @click="submit">
				<text class="submit-text">签 署 登 录</text>
			</view>

			<view class="foot-link">
				尚未订阅？<text class="link" @click="goChapter(chapters[3])">登记为读者</text>
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
				current: 0,
				account: '',
				password: '',
				showPassword: false,
				remember: true,
				agreed: false,
				focusField: ''
			}
		},
		methods: {
			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			goChapter(item) {
				if (item.idx === '01') return
				uni.redirectTo({
					url: item.path
				})
			},

			submit() {
				if (!this.agreed) return this.toast('请先阅读并同意读者协议')
				if (!this.account) return this.toast('请输入署名')
				if (!this.password) return this.toast('请输入登录口令')
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
