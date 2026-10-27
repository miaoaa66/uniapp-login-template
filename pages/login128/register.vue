<template>
	<view class="page">
		<view class="blob blob-1"></view>
		<view class="blob blob-2"></view>

		<view class="content">
			<view class="tray pop pop-1">
				<view class="tray-disc">
					<view class="tray-jelly"></view>
				</view>
				<view class="tray-shadow"></view>
				<view class="tray-name">果冻工坊</view>
				<view class="tray-sub">JELLY TRAY</view>
			</view>

			<view class="head pop pop-2">
				<view class="head-title">翻页捏糖</view>
				<view class="head-sub">像翻软糖图鉴：左右滑动卡片完成三步。</view>
			</view>

			<view class="deck-wrap pop pop-3">
				<view class="deck-track" :style="{ transform: 'translateX(' + (-step * 100) + '%)' }"
					@touchstart="onTouchStart" @touchend="onTouchEnd">
					<!-- 第一页：捏身份 -->
					<view class="deck-card">
						<text class="field-label">第一步 · 你是哪颗软糖？</text>
						<view class="identity-row">
							<view v-for="(item, i) in identities" :key="i" class="identity" :class="{ on: identity === item.key }" @click="identity = item.key">
								<text class="identity-emoji">{{ item.emoji }}</text>
								<text class="identity-name">{{ item.name }}</text>
							</view>
						</view>

						<view class="submit" @click="next">
							<text class="submit-text">翻到下一页</text>
						</view>

						<view class="foot-link">
							已有账号？<text class="link" @click="goLogin">回去登录</text>
						</view>
					</view>

					<!-- 第二页：填信息 -->
					<view class="deck-card">
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
							<text class="submit-text">翻到下一页</text>
						</view>
					</view>

					<!-- 第三页：完成 -->
					<view class="deck-card">
						<view class="identity-row" style="justify-content: center;">
							<view class="identity on" style="width: 200rpx; height: 200rpx;">
								<text class="identity-emoji" style="font-size: 96rpx;">🍮</text>
								<text class="identity-name">软糖出炉！</text>
							</view>
						</view>

						<view class="head-sub" style="text-align: center; margin-top: 24rpx;">
							身份「{{ identityName }}」· 昵称「{{ nickname || '未填' }}」已装盒
						</view>

						<view class="submit" @click="submit">
							<text class="submit-text">完成注册</text>
						</view>
					</view>
				</view>

				<view class="dots">
					<view v-for="i in 3" :key="i" class="dot" :class="{ on: step === i - 1 }"></view>
				</view>
			</view>

			<view class="step-tip">{{ stepTip }}</view>

			<view class="agreement" v-if="step !== 0">
				<view class="checkbox" :class="{ on: agreed }" @click="agreed = !agreed"></view>
				<text>我已阅读并同意<text class="link">《用户协议》</text>与<text class="link">《隐私政策》</text></text>
			</view>

			<view class="foot-note pop pop-4">© 2026 JELLY TRAY · 果冻工坊</view>
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
				focusField: '',
				startX: 0
			}
		},
		computed: {
			stepTip() {
				return ['第 1 页 / 共 3 页 · 可左右滑动', '第 2 页 / 共 3 页 · 右滑可返回', '第 3 页 / 共 3 页 · 装盒完成'][this.step]
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

			/* ---------- 手势滑动翻页 ---------- */
			onTouchStart(e) {
				this.startX = e.touches[0].clientX
			},

			onTouchEnd(e) {
				const dx = e.changedTouches[0].clientX - this.startX
				if (dx < -50) {
					this.next()
				} else if (dx > 50 && this.step > 0) {
					this.step--
				}
			},

			next() {
				if (this.step === 0) {
					if (!this.identity) return this.toast('先挑一颗软糖身份')
				} else if (this.step === 1) {
					if (!this.nickname) return this.toast('请输入昵称')
					if (!this.account) return this.toast('请输入手机号或邮箱')
					if (!this.password || this.password.length < 8) return this.toast('密码不少于 8 位')
					if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				}
				if (this.step < 2) this.step++
			},

			submit() {
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端注册接口，成功后返回登录页
				this.toast('软糖出炉 · 注册成功')
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
