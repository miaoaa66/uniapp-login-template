<template>
	<view class="tw-page">
		<!-- 一张竖放信纸 -->
		<view class="tw-paper">
			<!-- 回形针返回键 -->
			<view class="tw-clip" @click="goBack"><view class="tw-clip__inner"></view></view>

			<!-- 信头：双横线之间 认证函 -->
			<view class="tw-head">
				<view class="tw-head__line"></view>
				<view class="tw-head__mid">
					<text class="tw-head__en">CERTIFICATION</text>
					<text class="tw-head__cn">认 证 函</text>
				</view>
				<view class="tw-head__line"></view>
			</view>

			<!-- 书信空栏 -->
			<view class="tw-lines">
				<block v-if="tab === 0">
					<view class="tw-line">
						<text class="tw-line__label">账号</text>
						<view class="tw-line__blank">
							<input class="tw-line__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#b7b0a1" maxlength="40" />
						</view>
					</view>
					<view class="tw-line">
						<text class="tw-line__label">密码</text>
						<view class="tw-line__blank">
							<input class="tw-line__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#b7b0a1" maxlength="20" />
							<view class="tw-line__eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
					</view>
				</block>
				<block v-else-if="tab === 1">
					<view class="tw-line">
						<text class="tw-line__label">手机</text>
						<view class="tw-line__blank">
							<input class="tw-line__in" v-model="phone" type="number" placeholder="11 位手机号" placeholder-style="color:#b7b0a1" maxlength="11" />
						</view>
					</view>
					<view class="tw-line">
						<text class="tw-line__label">印文</text>
						<view class="tw-line__blank">
							<input class="tw-line__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#b7b0a1" maxlength="6" />
							<text class="tw-word" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</text>
						</view>
					</view>
				</block>
				<block v-else>
					<view class="tw-line">
						<text class="tw-line__label">邮箱</text>
						<view class="tw-line__blank">
							<input class="tw-line__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#b7b0a1" maxlength="40" />
						</view>
					</view>
					<view class="tw-line">
						<text class="tw-line__label">印文</text>
						<view class="tw-line__blank">
							<input class="tw-line__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#b7b0a1" maxlength="6" />
							<text class="tw-word" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</text>
						</view>
					</view>
				</block>
			</view>

			<!-- 复写纸选项 = 登录方式切换 -->
			<view class="tw-carbon">
				<view v-for="(item, i) in tabs" :key="i" class="tw-carbon__opt" :class="{ on: tab === i }" @click="tab = i">
					<view class="tw-box"><view class="tw-box__x"></view></view>
					<text>{{ item }}</text>
				</view>
			</view>

			<!-- 信末：勾选项 + 此致敬礼 -->
			<view class="tw-post">
				<view class="tw-agree" @click="agreed = !agreed">
					<view class="tw-check" :class="{ on: agreed }"></view>
					<text>我已阅读并同意<text class="tw-link">《用户协议》</text>与<text class="tw-link">《隐私政策》</text></text>
				</view>
				<text class="tw-post__zh">此致</text>
				<text class="tw-post__jing">敬礼！</text>
				<text class="tw-post__date">PIXEL TYPEWRITER CO. · 1983</text>
			</view>

			<!-- 打字机大圆键 + 色带 -->
			<view class="tw-keywrap">
				<view class="tw-ribbon" :class="{ go: ribbon }"></view>
				<view class="tw-key" @click="onSend"><text class="tw-key__t">寄 出</text></view>
			</view>

			<view class="tw-foot">
				<text class="tw-foot__link" @click="goForget">忘记密码？</text>
				<text class="tw-foot__sep"> · </text>
				<text class="tw-foot__link" @click="goRegister">还没有账户？去登记</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabs: ["密码", "短信", "邮箱"],
				tab: 0,
				ribbon: false,
				ribbonTimer: null,
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
			if (this.ribbonTimer) clearTimeout(this.ribbonTimer)
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

			// 色带从左向右横扫一次
			fireRibbon() {
				this.ribbon = false
				if (this.ribbonTimer) clearTimeout(this.ribbonTimer)
				setTimeout(() => { this.ribbon = true }, 30)
				this.ribbonTimer = setTimeout(() => { this.ribbon = false }, 800)
			},

			onSend() {
				this.fireRibbon()
				this.submit()
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
				uni.navigateTo({ url: '/pages/login98/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login98/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
