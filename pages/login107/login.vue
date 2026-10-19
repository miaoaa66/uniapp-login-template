<template>
	<view class="page">
		<view class="paper">
			<!-- 报头 -->
			<view class="masthead-meta">
				<text>第 107 期</text>
				<text>2026 年 10 月 19 日 · 星期一</text>
				<text>零售价 壹圆</text>
			</view>
			<view class="rule-double"></view>
			<view class="brand-line">
				<text class="brand">晨报</text>
				<text class="brand-en">THE MORNING POST</text>
			</view>
			<view class="rule-thin"></view>

			<!-- 头版 -->
			<view class="kicker">头版 · 用户登录</view>
			<view class="headline">欢迎回到编辑部</view>
			<view class="deck">请签署你的署名与口令，本期《晨报》将继续为你送达。所有登录记录均按档案规范归档。</view>

			<view class="form">
				<view class="field">
					<text class="label">署名 / ACCOUNT</text>
					<view class="field-line">
						<input class="input" v-model="account" placeholder="手机号 / 邮箱 / 工号" placeholder-class="ph" maxlength="40" />
					</view>
				</view>

				<view class="field">
					<text class="label">口令 / PASSWORD</text>
					<view class="field-line">
						<input class="input" v-model="password" :password="!showPassword" placeholder="请输入登录口令" placeholder-class="ph" maxlength="20" />
						<text class="toggle" @click="showPassword = !showPassword">{{ showPassword ? '隐去' : '显露' }}</text>
					</view>
				</view>

				<view class="row-between">
					<view class="remember" @click="remember = !remember">
						<view class="checkbox" :class="{ on: remember }"></view>
						<text>记住此设备</text>
					</view>
					<text class="link" @click="goForget">忘记口令？</text>
				</view>

				<view class="submit" @click="submit">
					<text class="submit-text">签 署 登 录</text>
					<view class="stamp">核准</view>
				</view>

				<view class="alt-rule">
					<view class="line"></view>
					<text class="txt">其他签署方式</text>
					<view class="line"></view>
				</view>

				<view class="alt-row">
					<view class="alt-btn" @click="altLogin('验证码')">短信验证码</view>
					<view class="alt-btn" @click="altLogin('邮箱')">邮箱链接</view>
				</view>

				<view class="foot-link">
					尚未订阅？<text class="link" @click="goRegister">登记为读者</text>
				</view>

				<view class="agreement">
					<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
					<text>本人已阅读并同意<text class="link">《读者协议》</text>与<text class="link">《信息保密条款》</text></text>
				</view>
			</view>

			<view class="colophon">
				<text>A1 版 · 用户中心</text>
				<text>本报编辑部</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				account: '',
				password: '',
				showPassword: false,
				remember: true,
				agreed: false
			}
		},
		methods: {
			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			submit() {
				if (!this.agreed) return this.toast('请先阅读并同意读者协议')
				if (!this.account) return this.toast('请输入署名')
				if (!this.password) return this.toast('请输入登录口令')
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			altLogin(type) {
				this.toast(type + '登录开发中')
			},

			goRegister() {
				uni.navigateTo({
					url: '/pages/login107/register'
				})
			},

			goForget() {
				uni.navigateTo({
					url: '/pages/login107/forget'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
