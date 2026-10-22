<template>
	<view class="am-page">
		<!-- 双线框头部 -->
		<view class="am-head">
			<view class="am-head__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="am-head__t">PIXEL BBS · 口令恢复</text>
			<text class="am-head__r">passwd</text>
		</view>

		<view class="am-body">
			<text class="am-cap">PASSWORD RECOVERY / 口令恢复协议</text>

			<!-- 协议步骤行 -->
			<view class="am-steps">
				<view class="am-steps__row" :class="{ on: step >= 1 }">
					<text class="am-steps__t">STEP 1 验证身份</text>
					<text class="am-steps__ok">{{ step >= 2 ? 'OK' : '...' }}</text>
				</view>
				<view class="am-steps__row" :class="{ on: step >= 2 }">
					<text class="am-steps__t">STEP 2 重置密码</text>
					<text class="am-steps__ok">{{ done ? 'OK' : (step >= 2 ? 'RUN' : '...') }}</text>
				</view>
			</view>

			<!-- STEP 1：邮箱 + 6 格琥珀方块验证码 -->
			<view class="am-fields">
				<view class="am-f">
					<text class="am-f__k">MAIL / 注册邮箱</text>
					<view class="am-f__row">
						<input class="am-f__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#6b520e" maxlength="40" />
						<text class="am-f__send" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '[SEND]' }}</text>
						<text class="am-caret">▉</text>
					</view>
				</view>

				<view class="am-cells">
					<input v-for="(c, i) in cells" :key="i" class="am-cell" :class="{ on: !!c }"
						v-model="cells[i]" type="number" maxlength="1" placeholder=" " placeholder-style="color:#3a2c08"
						:focus="focusIndex === i" @input="onCell(i, $event)" />
				</view>
				<text class="am-tip">INPUT 6-DIGIT CODE / 输入 6 位验证码</text>

				<!-- STEP 2：新密码两行 -->
				<view class="am-f">
					<text class="am-f__k">NEWPASS / 新口令</text>
					<view class="am-f__row">
						<input class="am-f__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#6b520e" maxlength="20" />
						<view class="am-f__eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						<text class="am-caret">▉</text>
					</view>
				</view>

				<view class="am-f">
					<text class="am-f__k">CONFIRM / 确认口令</text>
					<view class="am-f__row">
						<input class="am-f__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#6b520e" maxlength="20" />
						<text class="am-caret">▉</text>
					</view>
				</view>

				<view class="am-f am-f--lnk">
					<text class="am-f__lnk" @click="goLogin">想起口令了？[LOGIN]</text>
				</view>
			</view>

			<!-- 完成反白大字 -->
			<view v-if="done" class="am-done">
				<text>** PASSWORD UPDATED **</text>
			</view>
		</view>

		<!-- 底部反色状态栏 -->
		<view class="am-fbar">
			<text class="am-fbar__l">[F1 帮助]</text>
			<text class="am-fbar__c">PIXEL BBS v3.2</text>
			<view class="am-enter" @click="submit">
				<text>[ENTER 确认]</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				step: 1,
				done: false,
				showPwd: false,
				email: '',
				cells: ['', '', '', '', '', ''],
				focusIndex: 0,
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null
			}
		},
		onLoad(options) {
			if (options && options.demo) {
				this.email = 'player@pixel.id'
				this.cells = ['1', '9', '8', '3', '', '']
				this.password = '1up-1983'
				this.password2 = '1up-1983'
			}
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			toast(title) {
				uni.showToast({ title, icon: 'none' })
			},

			isEmail(v) {
				return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v)
			},

			// 输入后自动跳下一格
			onCell(i, e) {
				const v = (e.detail && e.detail.value) || ''
				if (v && i < 5) this.focusIndex = i + 1
			},

			sendCode() {
				if (this.mailLeft > 0) return
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				this.mailLeft = 60
				this.focusIndex = 0
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发送')
				if (this.timer) return
				this.timer = setInterval(() => {
					if (this.mailLeft > 0) this.mailLeft--
					else this.clearTimer()
				}, 1000)
			},

			clearTimer() {
				if (this.timer) {
					clearInterval(this.timer)
					this.timer = null
				}
			},

			// 保持原校验顺序；STEP 标记随校验推进
			submit() {
				if (!this.isEmail(this.email)) {
					this.step = 1
					return this.toast('请输入正确的邮箱')
				}
				if (this.cells.join('').length < 6) {
					this.step = 1
					return this.toast('请输入 6 位验证码')
				}
				this.step = 2
				if (this.password.length < 8) return this.toast('新密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				// TODO: 调用后端重置密码接口
				this.done = true
				this.toast('密码已重置')
			},

			goBack() {
				uni.navigateBack()
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
