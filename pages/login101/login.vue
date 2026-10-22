<template>
	<view class="nt-page">
		<view class="nt-boardpin nt-boardpin--lt"></view>
		<view class="nt-boardpin nt-boardpin--rt"></view>
		<view class="nt-boardpin nt-boardpin--lb"></view>
		<view class="nt-boardpin nt-boardpin--rb"></view>

		<view class="nt-tag" @click="goBack">
			<view class="nt-tag__pin"></view>
			<view class="pxi pxi-back"></view>
			<text class="nt-tag__t">任务板 · 登录</text>
		</view>

		<view :key="tab" class="nt-note nt-note--big" :class="'nt-note--c' + tab">
			<view class="nt-pin"></view>
			<text class="nt-note__title">{{ tabs[tab] }}</text>

			<view class="nt-form">
				<block v-if="tab === 0">
					<view class="nt-line">
						<text class="nt-line__k">账号</text>
						<input class="nt-line__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#8a6a4f" maxlength="40" />
					</view>
					<view class="nt-line">
						<text class="nt-line__k">密码</text>
						<input class="nt-line__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#8a6a4f" maxlength="20" />
						<view class="nt-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>
					<view class="nt-mini"><text class="nt-mini__t" @click="goForget">忘记密码？换一张备忘 →</text></view>
				</block>
				<block v-else-if="tab === 1">
					<view class="nt-line">
						<text class="nt-line__k">手机号</text>
						<input class="nt-line__in" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#8a6a4f" maxlength="11" />
					</view>
					<view class="nt-line">
						<text class="nt-line__k">验证码</text>
						<input class="nt-line__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#8a6a4f" maxlength="6" />
						<text class="nt-send" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '取验证码' }}</text>
					</view>
				</block>
				<block v-else>
					<view class="nt-line">
						<text class="nt-line__k">邮箱</text>
						<input class="nt-line__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#8a6a4f" maxlength="40" />
					</view>
					<view class="nt-line">
						<text class="nt-line__k">验证码</text>
						<input class="nt-line__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#8a6a4f" maxlength="6" />
						<text class="nt-send" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '取验证码' }}</text>
					</view>
				</block>

				<view class="nt-agree" @click="agreed = !agreed">
					<view class="nt-check" :class="{ on: agreed }"></view>
					<text class="nt-agree__t">我已阅读并同意<text class="nt-agree__lnk">《用户协议》</text>与<text class="nt-agree__lnk">《隐私政策》</text></text>
				</view>

				<view class="nt-tapebtn" @click="submit">
					<text class="nt-tapebtn__t">按 下 提 交</text>
				</view>
			</view>
		</view>

		<view class="nt-thumbrow">
			<view v-for="item in thumbTabs" :key="item.i" class="nt-thumb" :class="'nt-thumb--c' + item.i" @click="tab = item.i">
				<view class="nt-thumb__pin"></view>
				<text class="nt-thumb__t">{{ item.label }}</text>
			</view>
		</view>

		<view class="nt-scrap" @click="goRegister">
			<text class="nt-scrap__t">还没有便签？写一张新的 → 立即注册</text>
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
		computed: {
			// 未选中的两张便签 → 缩略行
			thumbTabs() {
				return this.tabs.map((label, i) => ({ i, label })).filter(t => t.i !== this.tab)
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
				uni.navigateTo({ url: '/pages/login101/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login101/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
