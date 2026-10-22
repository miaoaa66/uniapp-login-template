<template>
	<view class="ts-page">
		<view class="ts-device">
			<view class="ts-screw ts-screw--lt"></view>
			<view class="ts-screw ts-screw--rt"></view>
			<view class="ts-screw ts-screw--lb"></view>
			<view class="ts-screw ts-screw--rb"></view>

			<view class="ts-toprow">
				<view class="ts-back" @click="goBack">
					<view class="pxi pxi-back"></view>
				</view>
				<text class="ts-brandt">PIXELMAN · 登录</text>
				<view class="ts-led"></view>
			</view>

			<view class="ts-bay">
				<view class="ts-win">
					<view class="ts-reel ts-reel--l"></view>
					<view class="ts-eq">
						<view class="ts-eq__b ts-eq__b--0"></view>
						<view class="ts-eq__b ts-eq__b--1"></view>
						<view class="ts-eq__b ts-eq__b--2"></view>
						<view class="ts-eq__b ts-eq__b--3"></view>
						<view class="ts-eq__b ts-eq__b--4"></view>
					</view>
					<view class="ts-reel ts-reel--r"></view>
				</view>
				<view class="ts-winlabel">
					<text class="ts-winlabel__t">SIDE A · PIXEL MIX</text>
				</view>
			</view>

			<view class="ts-presets">
				<view v-for="(item, i) in tabs" :key="i" class="ts-preset" :class="{ on: tab === i }" @click="tab = i">
					<view class="ts-preset__led"></view>
					<view class="ts-preset__col">
						<text class="ts-preset__t">{{ presets[i] }}</text>
						<text class="ts-preset__cn">{{ item }}</text>
					</view>
				</view>
			</view>

			<view class="ts-list">
				<block v-if="tab === 0">
					<view class="ts-track">
						<text class="ts-track__no">TRACK 01</text>
						<text class="ts-track__name">账号</text>
						<input class="ts-track__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#8b939c" maxlength="40" />
					</view>
					<view class="ts-track">
						<text class="ts-track__no">TRACK 02</text>
						<text class="ts-track__name">密码</text>
						<input class="ts-track__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#8b939c" maxlength="20" />
						<view class="ts-hold" :class="{ on: showPwd }" @click="showPwd = !showPwd"><text class="ts-hold__t">HOLD</text></view>
					</view>
					<view class="ts-mini"><text class="ts-mini__t" @click="goForget">忘记密码？REW ◀◀ 倒带重置</text></view>
				</block>
				<block v-else-if="tab === 1">
					<view class="ts-track">
						<text class="ts-track__no">TRACK 01</text>
						<text class="ts-track__name">手机号</text>
						<input class="ts-track__in" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#8b939c" maxlength="11" />
					</view>
					<view class="ts-track">
						<text class="ts-track__no">TRACK 02</text>
						<text class="ts-track__name">验证码</text>
						<input class="ts-track__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#8b939c" maxlength="6" />
						<view class="ts-rec" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">
							<view class="ts-rec__dot"></view>
							<text class="ts-rec__t">{{ smsLeft > 0 ? smsLeft + 's' : 'REC' }}</text>
						</view>
					</view>
				</block>
				<block v-else>
					<view class="ts-track">
						<text class="ts-track__no">TRACK 01</text>
						<text class="ts-track__name">邮箱</text>
						<input class="ts-track__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#8b939c" maxlength="40" />
					</view>
					<view class="ts-track">
						<text class="ts-track__no">TRACK 02</text>
						<text class="ts-track__name">验证码</text>
						<input class="ts-track__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#8b939c" maxlength="6" />
						<view class="ts-rec" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">
							<view class="ts-rec__dot"></view>
							<text class="ts-rec__t">{{ mailLeft > 0 ? mailLeft + 's' : 'REC' }}</text>
						</view>
					</view>
				</block>

				<view class="ts-keyrow">
					<view class="ts-keycol">
						<view class="ts-agree" @click="agreed = !agreed">
							<view class="ts-check" :class="{ on: agreed }"></view>
							<text class="ts-agree__t">已阅读并同意<text class="ts-agree__lnk">《用户协议》</text>与<text class="ts-agree__lnk">《隐私政策》</text></text>
						</view>
						<view class="ts-foot">
							<text class="ts-foot__t">还没有磁带？</text>
							<text class="ts-foot__link" @click="goRegister">录一盘 → 立即注册</text>
						</view>
					</view>
					<view class="ts-play" @click="submit">
						<view class="pxi pxi-play ts-play__ico"></view>
						<text class="ts-play__t">PLAY</text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabs: ["密码登录","手机验证码","邮箱验证码"],
				presets: ["NORMAL", "SMS", "MAIL"],
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
				uni.navigateTo({ url: '/pages/login102/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login102/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
