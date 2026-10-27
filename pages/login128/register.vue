<template>
	<view class="page">
		<view class="blob blob-1"></view>
		<view class="blob blob-2"></view>

		<view class="content">
			<view class="studio pop pop-1">
				<view class="studio-logo"></view>
				<view>
					<view class="studio-name">黏土工坊</view>
					<view class="studio-sub">CLAY STUDIO</view>
				</view>
			</view>

			<view class="head pop pop-2">
				<view class="head-title">捏个新账号</view>
				<view class="head-sub">分三步，捏出属于你的黏土小人。</view>
			</view>

			<view class="steps pop pop-3">
				<view class="step-dot" :class="dotClass(0)">{{ step > 0 ? '✓' : '1' }}</view>
				<view class="step-bar" :class="{ fill: step >= 1 }"></view>
				<view class="step-dot" :class="dotClass(1)">{{ step > 1 ? '✓' : '2' }}</view>
				<view class="step-bar" :class="{ fill: step >= 2 }"></view>
				<view class="step-dot" :class="dotClass(2)">{{ step > 2 ? '✓' : '3' }}</view>
			</view>
			<view class="step-tip">{{ stepTip }}</view>

			<view class="card pop pop-3">
				<!-- 第一步：捏身份 -->
				<view v-if="step === 0">
					<text class="field-label">你是哪一块黏土？</text>
					<view class="identity-row">
						<view v-for="(item, i) in identities" :key="i" class="identity" :class="{ on: identity === item.key }" @click="identity = item.key">
							<text class="identity-emoji">{{ item.emoji }}</text>
							<text class="identity-name">{{ item.name }}</text>
						</view>
					</view>

					<view class="submit" @click="next">
						<text class="submit-text">捏好了，下一步</text>
					</view>

					<view class="foot-link">
						已有账号？<text class="link" @click="goLogin">返回登录</text>
					</view>
				</view>

				<!-- 第二步：填信息 -->
				<view v-else-if="step === 1">
					<view class="field">
						<text class="field-label">昵称</text>
						<view class="field-box" :class="{ active: focusField === 'nickname' }">
							<input class="input" v-model="nickname" placeholder="给自己起个名字" placeholder-class="ph" maxlength="20"
								@focus="focusField = 'nickname'" @blur="focusField = ''" />
						</view>
					</view>

					<view class="field">
						<text class="field-label">手机号 / 邮箱</text>
						<view class="field-box" :class="{ active: focusField === 'account' }">
							<input class="input" v-model="account" placeholder="用于登录与找回密码" placeholder-class="ph" maxlength="40"
								@focus="focusField = 'account'" @blur="focusField = ''" />
						</view>
					</view>

					<view class="field">
						<text class="field-label">设置密码</text>
						<view class="field-box" :class="{ active: focusField === 'password' }">
							<input class="input" v-model="password" :password="!showPassword" placeholder="8-20 位，建议字母加数字" placeholder-class="ph" maxlength="20"
								@focus="focusField = 'password'" @blur="focusField = ''" />
							<text class="eye" @click="showPassword = !showPassword">{{ showPassword ? '隐藏' : '显示' }}</text>
						</view>
					</view>

					<view class="field">
						<text class="field-label">确认密码</text>
						<view class="field-box" :class="{ active: focusField === 'password2' }">
							<input class="input" v-model="password2" :password="!showPassword" placeholder="请再次输入密码" placeholder-class="ph" maxlength="20"
								@focus="focusField = 'password2'" @blur="focusField = ''" />
						</view>
					</view>

					<view class="submit" @click="next">
						<text class="submit-text">捏好了，下一步</text>
					</view>
				</view>

				<!-- 第三步：完成 -->
				<view v-else>
					<view class="identity-row" style="justify-content: center;">
						<view class="identity on" style="width: 200rpx; height: 200rpx;">
							<text class="identity-emoji" style="font-size: 96rpx;">🎉</text>
							<text class="identity-name">捏合完成！</text>
						</view>
					</view>

					<view class="head-sub" style="text-align: center; margin-top: 24rpx;">
						身份「{{ identityName }}」· 昵称「{{ nickname || '未填' }}」已就绪
					</view>

					<view class="submit" @click="submit">
						<text class="submit-text">完成注册</text>
					</view>
				</view>

				<view class="agreement" v-if="step !== 0">
					<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
					<text>我已阅读并同意<text class="link">《用户协议》</text>与<text class="link">《隐私政策》</text></text>
				</view>
			</view>

			<view class="foot-note pop pop-4">© 2026 CLAY STUDIO · 黏土工坊</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				step: 0,
				identities: [
					{ key: 'student', name: '学生', emoji: '🎒' },
					{ key: 'creator', name: '创作者', emoji: '🎨' },
					{ key: 'guest', name: '访客', emoji: '🍃' }
				],
				identity: '',
				nickname: '',
				account: '',
				password: '',
				password2: '',
				showPassword: false,
				agreed: false,
				focusField: ''
			}
		},
		computed: {
			stepTip() {
				return ['第一步 · 选择你的黏土身份', '第二步 · 填写你的信息', '第三步 · 捏合完成'][this.step]
			},
			identityName() {
				const found = this.identities.find(i => i.key === this.identity)
				return found ? found.name : '未选择'
			}
		},
		methods: {
			toast(title) {
				uni.showToast({
					title,
					icon: 'none'
				})
			},

			dotClass(i) {
				if (i < this.step) return 'done'
				if (i === this.step) return 'on'
				return ''
			},

			next() {
				if (this.step === 0) {
					if (!this.identity) return this.toast('请先选择一个身份')
				} else if (this.step === 1) {
					if (!this.nickname) return this.toast('请输入昵称')
					if (!this.account) return this.toast('请输入手机号或邮箱')
					if (!this.password) return this.toast('请设置密码')
					if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				}
				this.step++
			},

			submit() {
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端注册接口，成功后返回登录页
				this.toast('注册成功')
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
