<template>
	<view class="page">
		<view class="paper">
			<view class="masthead-meta">
				<text>第 107 期 · 附刊</text>
				<text>2026 年 10 月 19 日</text>
				<text>读者登记处</text>
			</view>
			<view class="rule-double"></view>
			<view class="brand-line">
				<text class="brand">晨报</text>
				<text class="brand-en">THE MORNING POST</text>
			</view>
			<view class="rule-thin"></view>

			<view class="kicker">登记 · 新读者</view>
			<view class="headline">登记成为本报读者</view>
			<view class="deck">填写下列信息，编辑部将在核对后为你开通阅读权限，每期头版将准时送达。</view>

			<view class="form">
				<view class="field">
					<text class="label">署名 / NAME</text>
					<view class="field-line">
						<input class="input" v-model="nickname" placeholder="请输入你的署名" placeholder-class="ph" maxlength="20" />
					</view>
				</view>

				<view class="field">
					<text class="label">投递地址 / CONTACT</text>
					<view class="field-line">
						<input class="input" v-model="contact" placeholder="手机号 / 邮箱" placeholder-class="ph" maxlength="40" />
					</view>
				</view>

				<view class="field">
					<text class="label">设置口令 / PASSWORD</text>
					<view class="field-line">
						<input class="input" v-model="password" :password="!showPassword" placeholder="8-20 位，建议含字母与数字" placeholder-class="ph" maxlength="20" />
						<text class="toggle" @click="showPassword = !showPassword">{{ showPassword ? '隐去' : '显露' }}</text>
					</view>
				</view>

				<view class="field">
					<text class="label">确认口令 / CONFIRM</text>
					<view class="field-line">
						<input class="input" v-model="password2" :password="!showPassword" placeholder="请再次输入口令" placeholder-class="ph" maxlength="20" />
					</view>
				</view>

				<view class="submit" @click="submit">
					<text class="submit-text">提 交 登 记</text>
					<view class="stamp">收讫</view>
				</view>

				<view class="foot-link">
					已有订阅？<text class="link" @click="goLogin">返回签署</text>
				</view>

				<view class="agreement">
					<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
					<text>本人已阅读并同意<text class="link">《读者协议》</text>与<text class="link">《信息保密条款》</text></text>
				</view>
			</view>

			<view class="colophon">
				<text>A2 版 · 读者登记</text>
				<text>本报编辑部</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				nickname: '',
				contact: '',
				password: '',
				password2: '',
				showPassword: false,
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
				if (!this.nickname) return this.toast('请输入署名')
				if (!this.contact) return this.toast('请输入投递地址')
				if (!this.password) return this.toast('请设置口令')
				if (this.password !== this.password2) return this.toast('两次输入的口令不一致')
				if (!this.agreed) return this.toast('请先阅读并同意读者协议')
				// TODO: 调用后端注册接口，成功后返回登录页
				this.toast('登记成功')
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
