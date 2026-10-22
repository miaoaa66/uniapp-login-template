<template>
	<view class="vd-page">
		<!-- 机身：顶盖灯箱 + 橱窗 + 操作面板 -->
		<view class="vd-machine">
			<view class="vd-cap">
				<view class="vd-cap__back" @click="goBack"><view class="pxi pxi-back"></view></view>
				<view class="vd-sign">
					<view class="vd-sign__lamp"></view>
					<text class="vd-sign__t">PIXEL 贩卖机 · 24H</text>
					<view class="vd-sign__lamp vd-sign__lamp--r"></view>
				</view>
			</view>

			<!-- 玻璃橱窗：3 个货道 = 3 种登录方式 -->
			<view class="vd-window">
				<view class="vd-window__tube"></view>
				<view class="vd-slots">
					<view v-for="(s, i) in slots" :key="i" class="vd-slot" :class="{ on: tab === i, dim: tab > -1 && tab !== i }" @click="tab = i">
						<view class="vd-bag" :class="'vd-bag--' + i">
							<view class="vd-bag__seal"></view>
							<view class="vd-bag__ico"><view class="pxi" :class="s.icon"></view></view>
						</view>
						<text class="vd-slot__name">{{ s.name }}</text>
						<text class="vd-slot__price">¥0.00</text>
						<text class="vd-slot__no">{{ s.no }}</text>
						<view class="vd-slot__sel" :class="{ lit: tab === i }"><text>SELECTED</text></view>
					</view>
				</view>
			</view>

			<!-- 操作面板 -->
			<view class="vd-panel">
				<view class="vd-panel__row">
					<view class="vd-screen"><text class="vd-screen__t">{{ tab > -1 ? slots[tab].code : 'READY' }}</text></view>
					<view class="vd-coin"><view class="vd-coin__slit"></view><text class="vd-coin__t">COIN</text></view>
				</view>

				<view v-if="tab > -1" class="vd-form">
					<block v-if="tab === 0">
						<view class="vd-line">
							<text class="vd-line__tag">账号</text>
							<input class="vd-line__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#8b97a8" maxlength="40" />
						</view>
						<view class="vd-line">
							<text class="vd-line__tag">密码</text>
							<input class="vd-line__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#8b97a8" maxlength="20" />
							<view class="vd-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
						<view class="vd-row"><text class="vd-row__link" @click="goForget">忘记密码？呼叫服务</text></view>
					</block>
					<block v-else-if="tab === 1">
						<view class="vd-line">
							<text class="vd-line__tag">手机</text>
							<input class="vd-line__in" v-model="phone" type="number" placeholder="11 位手机号" placeholder-style="color:#8b97a8" maxlength="11" />
						</view>
						<view class="vd-line">
							<text class="vd-line__tag">验证</text>
							<input class="vd-line__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#8b97a8" maxlength="6" />
							<view class="vd-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>
					<block v-else>
						<view class="vd-line">
							<text class="vd-line__tag">邮箱</text>
							<input class="vd-line__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#8b97a8" maxlength="40" />
						</view>
						<view class="vd-line">
							<text class="vd-line__tag">验证</text>
							<input class="vd-line__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#8b97a8" maxlength="6" />
							<view class="vd-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
						</view>
					</block>

					<view class="vd-btn" @click="submit"><text>出  货</text></view>

					<view class="vd-agree" @click="agreed = !agreed">
						<view class="vd-check" :class="{ on: agreed }"></view>
						<text>我已阅读并同意<text class="vd-link">《用户协议》</text>与<text class="vd-link">《隐私政策》</text></text>
					</view>

					<view class="vd-foot">
						<text>还没有会员卡？</text>
						<text class="vd-foot__link" @click="goRegister">补货员登记</text>
					</view>
				</view>
				<view v-else class="vd-hint"><text>点击橱窗里的商品，选择登录方式</text></view>

				<!-- 出货口挡板 -->
				<view class="vd-hatch" :class="{ open: dispensing }">
					<view class="vd-hatch__door"><text>{{ dispensing ? '出货中…' : 'PUSH' }}</text></view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				slots: [
					{ no: 'A1', name: '密码登录', code: 'PASSWORD', icon: 'pxi-lock' },
					{ no: 'A2', name: '短信验证', code: 'SMS CODE', icon: 'pxi-phone' },
					{ no: 'A3', name: '邮箱验证', code: 'MAIL CODE', icon: 'pxi-mail' }
				],
				tab: -1,
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
				timer: null,
				dispensing: false,
				dispTimer: null
			}
		},
		onLoad(options) {
			// 演示参数：?tab=1&demo=1
			if (options && options.tab) this.tab = Number(options.tab) || 0
			if (options && options.demo) {
				this.fillDemo()
				if (!(options && options.tab)) this.tab = 0
			}
		},
		onUnload() {
			this.clearTimer()
			if (this.dispTimer) {
				clearTimeout(this.dispTimer)
				this.dispTimer = null
			}
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
				// 校验通过：挡板打开出货，1s 后复位
				// TODO: 调用后端登录接口，成功后跳转首页
				this.dispensing = true
				this.dispTimer = setTimeout(() => {
					this.dispensing = false
					this.dispTimer = null
					this.toast('登录成功')
				}, 1000)
			},

			goBack() {
				uni.navigateBack()
			},

			goRegister() {
				uni.navigateTo({ url: '/pages/login105/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login105/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
