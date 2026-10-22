<template>
	<view class="am-page">
		<!-- 双线框头部 -->
		<view class="am-head">
			<view class="am-head__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="am-head__t">PIXEL BBS · 新用户注册</text>
			<text class="am-head__r">adduser</text>
		</view>

		<view class="am-body">
			<text class="am-cap">NEW ACCOUNT / 注册新账户</text>

			<!-- 编号字段菜单 -->
			<view class="am-fields">
				<view class="am-f">
					<view class="am-f__krow">
						<text class="am-f__no">01</text>
						<text class="am-f__k">NICKNAME / 昵称</text>
					</view>
					<view class="am-f__row">
						<input class="am-f__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#6b520e" maxlength="12" />
						<text class="am-caret">▉</text>
					</view>
				</view>

				<view class="am-f">
					<view class="am-f__krow">
						<text class="am-f__no">02</text>
						<text class="am-f__k">MAIL / 邮箱地址</text>
					</view>
					<view class="am-f__row">
						<input class="am-f__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#6b520e" maxlength="40" />
						<text class="am-caret">▉</text>
					</view>
				</view>

				<view class="am-f">
					<view class="am-f__krow">
						<text class="am-f__no">03</text>
						<text class="am-f__k">CODE / 邮箱验证码</text>
					</view>
					<view class="am-f__row">
						<input class="am-f__in am-f__in--num" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#6b520e" maxlength="6" />
						<text class="am-f__send" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '[SEND]' }}</text>
						<text class="am-caret">▉</text>
					</view>
				</view>

				<view class="am-f">
					<view class="am-f__krow">
						<text class="am-f__no">04</text>
						<text class="am-f__k">PASSWORD / 口令</text>
					</view>
					<view class="am-f__row">
						<input class="am-f__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#6b520e" maxlength="20" />
						<view class="am-f__eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						<text class="am-caret">▉</text>
					</view>
				</view>

				<view class="am-f">
					<view class="am-f__krow">
						<text class="am-f__no">05</text>
						<text class="am-f__k">CONFIRM / 确认口令</text>
					</view>
					<view class="am-f__row">
						<input class="am-f__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#6b520e" maxlength="20" />
						<text class="am-caret">▉</text>
					</view>
				</view>

				<!-- METER 强度行 -->
				<view class="am-meter">
					<text class="am-meter__k">METER [</text>
					<view class="am-meter__bars">
						<view v-for="n in 6" :key="n" class="am-meter__b" :class="{ on: meterLit >= n }"></view>
					</view>
					<text class="am-meter__r">] {{ meterText }}</text>
				</view>

				<view class="am-f am-f--lnk">
					<text class="am-f__lnk" @click="goLogin">已有账户？[LOGIN]</text>
				</view>

				<view class="am-agree" @click="agreed = !agreed">
					<text class="am-agree__box">[{{ agreed ? 'X' : '\u00A0' }}]</text>
					<text class="am-agree__t">我已阅读并同意《用户协议》与《隐私政策》</text>
				</view>
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
				agreed: false,
				showPwd: false,
				nickname: '',
				email: '',
				mailCode: '',
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null
			}
		},
		computed: {
			strength() {
				const v = this.password
				if (!v) return 0
				let s = 0
				if (v.length >= 8) s++
				if (/[a-zA-Z]/.test(v) && /\d/.test(v)) s++
				if (/[^a-zA-Z0-9]/.test(v)) s++
				return s
			},
			// METER：0/2/4/6 格阶梯点亮
			meterLit() {
				return [0, 2, 4, 6][this.strength]
			},
			meterText() {
				return ['--', '弱', '中', '强'][this.strength]
			}
		},
		onLoad(options) {
			if (options && options.demo) {
				this.agreed = true
				this.nickname = 'PLAYER-1'
				this.email = 'player@pixel.id'
				this.mailCode = '1983'
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

			sendCode() {
				if (this.mailLeft > 0) return
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				this.mailLeft = 60
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

			submit() {
				if (!this.nickname) return this.toast('请输入昵称')
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (!this.mailCode) return this.toast('请输入邮箱验证码')
				if (this.password.length < 8) return this.toast('密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端注册接口
				this.toast('注册成功')
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
