<template>
	<view class="page">
		<text class="spine">纸页之间 · 文学季刊</text>

		<view class="masthead">
			<view class="rule-double"></view>
			<view class="kicker">
				<text class="kicker-cn">VOL.127 · 读者登记</text>
				<text class="kicker-en">SUBSCRIBE</text>
			</view>
			<view class="rule-thin"></view>
		</view>

		<view class="head">
			<view class="title">成为本刊的读者</view>
			<view class="deck">登记一位新读者，仅需几笔。你的署名将被印在这一期的订阅读者名册上。</view>
		</view>

		<view class="toc">
			<view v-for="(item, i) in chapters" :key="i" class="toc-item" :class="{ on: i === current }" @click="goChapter(item)">
				<text class="toc-idx">{{ item.idx }}</text>{{ item.name }}
			</view>
		</view>

		<view class="form">
			<view class="field">
				<view class="label">
					<text>笔名</text>
					<text class="label-en">PEN NAME</text>
				</view>
				<view class="field-box" :class="{ active: focusField === 'nickname' }">
					<view class="field-line">
						<input class="input" v-model="nickname" placeholder="为自己取一个笔名" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'nickname'" @blur="focusField = ''" />
					</view>
				</view>
			</view>

			<view class="field">
				<view class="label">
					<text>通联</text>
					<text class="label-en">CONTACT</text>
				</view>
				<view class="field-box" :class="{ active: focusField === 'account' }">
					<view class="field-line">
						<input class="input" v-model="account" placeholder="手机号 / 邮箱" placeholder-class="ph" maxlength="40"
							@focus="focusField = 'account'" @blur="focusField = ''" />
					</view>
				</view>
			</view>

			<view class="field">
				<view class="label">
					<text>设置口令</text>
					<text class="label-en">PASSWORD</text>
				</view>
				<view class="field-box" :class="{ active: focusField === 'password' }">
					<view class="field-line">
						<input class="input" v-model="password" :password="!showPassword" placeholder="8-20 位，建议字母加数字" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password'" @blur="focusField = ''" />
						<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐去' : '显露' }}</text>
					</view>
				</view>
			</view>

			<view class="field">
				<view class="label">
					<text>确认口令</text>
					<text class="label-en">CONFIRM</text>
				</view>
				<view class="field-box" :class="{ active: focusField === 'password2' }">
					<view class="field-line">
						<input class="input" v-model="password2" :password="!showPassword" placeholder="请再次输入口令" placeholder-class="ph" maxlength="20"
							@focus="focusField = 'password2'" @blur="focusField = ''" />
					</view>
				</view>
			</view>

			<view class="submit" @click="submit">
				<text class="submit-text">完 成 登 记</text>
			</view>

			<view class="foot-link">
				已是读者？<text class="link" @click="goChapter(chapters[0])">返回登录</text>
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
				current: 3,
				nickname: '',
				account: '',
				password: '',
				password2: '',
				showPassword: false,
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
				if (item.idx === '04') return
				uni.redirectTo({
					url: item.path
				})
			},

			submit() {
				if (!this.nickname) return this.toast('请输入笔名')
				if (!this.account) return this.toast('请输入手机号或邮箱')
				if (!this.password) return this.toast('请设置口令')
				if (this.password !== this.password2) return this.toast('两次输入的口令不一致')
				if (!this.agreed) return this.toast('请先阅读并同意读者协议')
				// TODO: 调用后端注册接口，成功后返回登录页
				this.toast('登记成功')
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
