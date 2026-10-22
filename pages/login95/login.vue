<template>
	<view class="ac-page">
		<!-- 顶部 marquee 灯箱 -->
		<view class="ac-marquee">
			<view class="ac-marquee__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<view class="ac-bulbs">
				<view v-for="n in 4" :key="'l'+n" class="ac-bulb" :class="n % 2 ? 'ac-bulb--a' : 'ac-bulb--b'"></view>
			</view>
			<text class="ac-marquee__t">PIXEL ARCADE</text>
			<view class="ac-bulbs">
				<view v-for="n in 4" :key="'r'+n" class="ac-bulb" :class="n % 2 ? 'ac-bulb--b' : 'ac-bulb--a'"></view>
			</view>
		</view>

		<!-- 投币口 -->
		<view class="ac-slotrow">
			<text class="ac-slotrow__t">COIN SLOT</text>
			<view class="ac-slot" @click="insertCoin">
				<view v-if="coinAni" class="ac-coin"></view>
			</view>
		</view>

		<!-- 屏幕 -->
		<view class="ac-screen" @click="screenTap">
			<!-- attract：INSERT COIN -->
			<block v-if="phase === 'attract'">
				<text class="ac-insert">INSERT COIN</text>
				<text class="ac-credit" :class="{ bump: bumped }">CREDIT {{ credit }}</text>
				<view class="ac-hi">
					<text class="ac-hi__l">1P 004500</text>
					<text class="ac-hi__l">2P 012800</text>
				</view>
				<text class="ac-hint">TAP SCREEN / COIN SLOT</text>
			</block>

			<!-- select：SELECT MODE -->
			<block v-else-if="phase === 'select'">
				<text class="ac-scr__h">SELECT MODE</text>
				<view v-for="(item, i) in modes" :key="i" class="ac-mode" @click.stop="pickMode(i)">
					<view class="ac-mode__cur" :class="{ on: tab === i }"></view>
					<text class="ac-mode__t">{{ item }}</text>
				</view>
				<text class="ac-hint">JOYSTICK + A BUTTON</text>
			</block>

			<!-- form：表单 -->
			<block v-else-if="phase === 'form'">
				<block v-if="tab === 0">
					<view class="ac-f">
						<text class="ac-f__k">1P ACCOUNT</text>
						<input class="ac-f__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#5a5470" maxlength="40" />
					</view>
					<view class="ac-f">
						<text class="ac-f__k">PASSWORD</text>
						<input class="ac-f__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#5a5470" maxlength="20" />
						<view class="ac-f__eye" @click.stop="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>
					<view class="ac-f ac-f--lnk">
						<text class="ac-f__lnk" @click.stop="goForget">忘记密码？</text>
					</view>
				</block>
				<block v-else-if="tab === 1">
					<view class="ac-f">
						<text class="ac-f__k">2P PHONE</text>
						<input class="ac-f__in ac-f__in--num" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#5a5470" maxlength="11" />
					</view>
					<view class="ac-f">
						<text class="ac-f__k">SMS CODE</text>
						<input class="ac-f__in ac-f__in--num" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#5a5470" maxlength="6" />
						<text class="ac-f__send" :class="{ off: smsLeft > 0 }" @click.stop="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : 'SEND' }}</text>
					</view>
				</block>
				<block v-else>
					<view class="ac-f">
						<text class="ac-f__k">3P MAIL</text>
						<input class="ac-f__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#5a5470" maxlength="40" />
					</view>
					<view class="ac-f">
						<text class="ac-f__k">MAIL CODE</text>
						<input class="ac-f__in ac-f__in--num" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#5a5470" maxlength="6" />
						<text class="ac-f__send" :class="{ off: mailLeft > 0 }" @click.stop="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : 'SEND' }}</text>
					</view>
				</block>

				<view class="ac-f ac-f--lnk">
					<text class="ac-f__lnk" @click.stop="goRegister">{{ tabLinkText }}</text>
				</view>

				<view class="ac-agree" @click.stop="agreed = !agreed">
					<view class="ac-agree__box" :class="{ on: agreed }"></view>
					<text class="ac-agree__t">我已阅读并同意《用户协议》与《隐私政策》</text>
				</view>

				<text class="ac-push">PUSH START</text>
			</block>

			<text class="ac-credit-mini">CREDIT {{ credit }}</text>
		</view>

		<!-- 控制面板 -->
		<view class="ac-panel">
			<view class="ac-stick">
				<view class="ac-stick__ball"></view>
				<view class="ac-stick__rod"></view>
				<view class="ac-stick__base"></view>
			</view>
			<view class="ac-btns">
				<view class="ac-ab ac-ab--b" @click="pressB"><text>B</text></view>
				<view class="ac-ab ac-ab--a" @click="pressA"><text>A</text></view>
			</view>
		</view>

		<view class="ac-hud">
			<text class="ac-hud__l">1UP 004500</text>
			<text class="ac-hud__l">HIGH 012800</text>
			<text class="ac-hud__l">1994 © PIXEL</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 屏幕三阶段状态机：attract → select → form
				phase: 'attract',
				modes: ['1P PASSWORD LOGIN', '2P SMS VERIFY', '3P MAIL VERIFY'],
				credit: 0,
				coinAni: false,
				bumped: false,
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
			tabLinkText() {
				return this.tab === 0 ? '没有街机档案？去注册' : '还没有街机档案？立即注册'
			}
		},
		onLoad(options) {
			// 演示参数：?tab=1&demo=1（带 tab 直接从 select 开始）
			if (options && options.tab) {
				this.tab = Number(options.tab) || 0
				this.credit = 1
				this.phase = 'select'
			}
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

			// 投币：硬币下落动画 → CREDIT 跳动 → 进入 select
			insertCoin() {
				if (this.coinAni) return
				if (this.credit > 0) {
					if (this.phase === 'attract') this.phase = 'select'
					return
				}
				this.coinAni = true
				setTimeout(() => {
					this.coinAni = false
					this.credit = 1
					this.bumped = true
					setTimeout(() => {
						this.bumped = false
						this.phase = 'select'
					}, 450)
				}, 400)
			},

			// 点击屏幕：attract 阶段等同投币
			screenTap() {
				if (this.phase === 'attract') this.insertCoin()
			},

			pickMode(i) {
				this.tab = i
				this.phase = 'form'
			},

			// A 键：attract=投币 / select=确认当前行 / form=提交
			pressA() {
				if (this.phase === 'attract') return this.insertCoin()
				if (this.phase === 'select') return this.pickMode(this.tab)
				this.submit()
			},

			// B 键：form 退回 select
			pressB() {
				if (this.phase === 'form') this.phase = 'select'
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
				if (this.phase !== 'form') return
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
				uni.navigateTo({ url: '/pages/login95/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login95/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
