<template>
	<view class="am-page">
		<!-- 双线框头部 -->
		<view class="am-head">
			<view class="am-head__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="am-head__t">PIXEL BBS · 认证节点</text>
			<text class="am-head__r">tty0</text>
		</view>

		<view class="am-body">
			<text class="am-cap">SELECT SERVICE / 选择认证服务</text>

			<!-- 编号菜单 -->
			<view class="am-menu">
				<view class="am-menu__row" :class="{ on: tab === 0 }" @click="tab = 0">
					<text class="am-menu__arr">{{ tab === 0 ? '→' : ' ' }}</text>
					<text class="am-menu__no">1.</text>
					<text class="am-menu__t">密码登录</text>
					<text class="am-menu__st">{{ tab === 0 ? 'SEL' : 'READY' }}</text>
				</view>
				<view class="am-menu__row" :class="{ on: tab === 1 }" @click="tab = 1">
					<text class="am-menu__arr">{{ tab === 1 ? '→' : ' ' }}</text>
					<text class="am-menu__no">2.</text>
					<text class="am-menu__t">短信验证码</text>
					<text class="am-menu__st">{{ tab === 1 ? 'SEL' : 'READY' }}</text>
				</view>
				<view class="am-menu__row" :class="{ on: tab === 2 }" @click="tab = 2">
					<text class="am-menu__arr">{{ tab === 2 ? '→' : ' ' }}</text>
					<text class="am-menu__no">3.</text>
					<text class="am-menu__t">邮箱验证码</text>
					<text class="am-menu__st">{{ tab === 2 ? 'SEL' : 'READY' }}</text>
				</view>
			</view>

			<!-- 输入区：标签行 + 下划线空栏 -->
			<view class="am-fields">
				<block v-if="tab === 0">
					<view class="am-f">
						<text class="am-f__k">ACCOUNT / 账号</text>
						<view class="am-f__row">
							<input class="am-f__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#6b520e" maxlength="40" />
							<text class="am-caret">▉</text>
						</view>
					</view>
					<view class="am-f">
						<text class="am-f__k">PASSWORD / 口令</text>
						<view class="am-f__row">
							<input class="am-f__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#6b520e" maxlength="20" />
							<view class="am-f__eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
							<text class="am-caret">▉</text>
						</view>
					</view>
					<view class="am-f am-f--lnk">
						<text class="am-f__lnk" @click="goForget">忘记密码？[RESET]</text>
					</view>
				</block>
				<block v-else-if="tab === 1">
					<view class="am-f">
						<text class="am-f__k">PHONE / 短信号码</text>
						<view class="am-f__row">
							<input class="am-f__in am-f__in--num" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#6b520e" maxlength="11" />
							<text class="am-caret">▉</text>
						</view>
					</view>
					<view class="am-f">
						<text class="am-f__k">CODE / 短信验证码</text>
						<view class="am-f__row">
							<input class="am-f__in am-f__in--num" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#6b520e" maxlength="6" />
							<text class="am-f__send" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '[SEND]' }}</text>
							<text class="am-caret">▉</text>
						</view>
					</view>
				</block>
				<block v-else>
					<view class="am-f">
						<text class="am-f__k">MAIL / 邮箱地址</text>
						<view class="am-f__row">
							<input class="am-f__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#6b520e" maxlength="40" />
							<text class="am-caret">▉</text>
						</view>
					</view>
					<view class="am-f">
						<text class="am-f__k">CODE / 邮箱验证码</text>
						<view class="am-f__row">
							<input class="am-f__in am-f__in--num" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#6b520e" maxlength="6" />
							<text class="am-f__send" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '[SEND]' }}</text>
							<text class="am-caret">▉</text>
						</view>
					</view>
				</block>

				<view class="am-f am-f--lnk">
					<text class="am-f__lnk" @click="goRegister">尚无账户？[REGISTER]</text>
				</view>

				<!-- 协议：[X] 行 -->
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
				tabs: ["密码登录","手机验证码","邮箱验证码"],
				tab: 0,
				agreed: false,
				showPwd: false,
				account: '',
				password: '',
				phone: '',
				smsCode: '',
				email: '',
				mailCode: '',
				smsLeft: 0,
				mailLeft: 0,
				timer: null
			}
		},
		onLoad(options) {
			// 演示参数：?tab=1&demo=1
			if (options && options.tab) this.tab = Number(options.tab) || 0
			if (options && options.demo) this.fillDemo()
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			fillDemo() {
				this.agreed = true
				this.account = 'player@pixel.id'
				this.password = '1up-1983'
				this.phone = '13800001234'
				this.email = 'player@pixel.id'
				this.smsCode = '1983'
				this.mailCode = '1983'
			},

			toast(title) {
				uni.showToast({ title, icon: 'none' })
			},

			isPhone(v) {
				return /^1\d{10}$/.test(v)
			},

			isEmail(v) {
				return /^[\w.-]+@[\w-]+\.[\w.-]+$/.test(v)
			},

			sendCode(type) {
				if (type === 'sms') {
					if (this.smsLeft > 0) return
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
					this.smsLeft = 60
				} else {
					if (this.mailLeft > 0) return
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					this.mailLeft = 60
				}
				// TODO: 调用后端发送验证码接口
				this.toast('验证码已发送')
				this.startTimer()
			},

			startTimer() {
				if (this.timer) return
				this.timer = setInterval(() => {
					if (this.smsLeft > 0) this.smsLeft--
					if (this.mailLeft > 0) this.mailLeft--
					if (this.smsLeft <= 0 && this.mailLeft <= 0) this.clearTimer()
				}, 1000)
			},

			clearTimer() {
				if (this.timer) {
					clearInterval(this.timer)
					this.timer = null
				}
			},

			submit() {
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				if (this.tab === 0) {
					if (!this.account) return this.toast('请输入账号')
					if (!this.password) return this.toast('请输入登录密码')
				} else if (this.tab === 1) {
					if (!this.isPhone(this.phone)) return this.toast('请输入正确的手机号')
					if (!this.smsCode) return this.toast('请输入短信验证码')
				} else {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (!this.mailCode) return this.toast('请输入邮箱验证码')
				}
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goBack() {
				uni.navigateBack()
			},

			goRegister() {
				uni.navigateTo({ url: '/pages/login97/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login97/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
