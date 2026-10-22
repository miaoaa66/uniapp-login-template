<template>
	<view class="ca-page">
		<!-- 当月月历（纯展示，不可点） -->
		<view class="ca-cal">
			<view class="ca-cal__rings"><view class="ca-ring"></view><view class="ca-ring"></view></view>
			<view class="ca-cal__head">
				<view class="ca-cal__back" @click="goBack"><view class="pxi pxi-back"></view></view>
				<view class="ca-cal__title">
					<text class="ca-cal__cn">{{ calYear }}年{{ calMonth }}月</text>
					<text class="ca-cal__en">{{ monthEn }}</text>
				</view>
				<text class="ca-cal__today">{{ today }}</text>
			</view>
			<view class="ca-cal__week">
				<text v-for="(w, i) in week" :key="i" class="ca-cal__wd" :class="{ red: i === 0 || i === 6 }">{{ w }}</text>
			</view>
			<view class="ca-cal__grid">
				<view v-for="(c, i) in calCells" :key="i" class="ca-cal__cell" :class="{ red: c.col === 0 || c.col === 6, today: c.n === today }">
					<text v-if="c.n" class="ca-cal__num">{{ c.n }}</text>
					<view v-if="c.n === today && stamped" class="ca-stamp"></view>
				</view>
			</view>
		</view>

		<!-- 今日打卡面板 -->
		<view class="ca-panel">
			<view class="ca-panel__tear"></view>
			<text class="ca-panel__t">今日打卡</text>
			<text class="ca-panel__sub">选一枚打卡章，盖下今天的出勤</text>

			<view class="ca-seals">
				<view v-for="(s, i) in seals" :key="i" class="ca-seal" :class="{ on: tab === i }" @click="tab = i">
					<text class="ca-seal__no">0{{ i + 1 }}</text>
					<text class="ca-seal__t">{{ s }}</text>
				</view>
			</view>

			<view class="ca-form">
				<block v-if="tab === 0">
					<view class="ca-line">
						<text class="ca-line__tag">账号</text>
						<input class="ca-line__in" v-model="account" placeholder="手机号 / 邮箱" placeholder-style="color:#9a9a9a" maxlength="40" />
					</view>
					<view class="ca-line">
						<text class="ca-line__tag">密码</text>
						<input class="ca-line__in" v-model="password" :password="!showPwd" placeholder="请输入登录密码" placeholder-style="color:#9a9a9a" maxlength="20" />
						<view class="ca-line__eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>
					<view class="ca-row"><text class="ca-row__link" @click="goForget">忘记密码？翻到背面看看</text></view>
				</block>
				<block v-else-if="tab === 1">
					<view class="ca-line">
						<text class="ca-line__tag">手机</text>
						<input class="ca-line__in" v-model="phone" type="number" placeholder="11 位手机号" placeholder-style="color:#9a9a9a" maxlength="11" />
					</view>
					<view class="ca-line">
						<text class="ca-line__tag">验证</text>
						<input class="ca-line__in" v-model="smsCode" type="number" placeholder="短信验证码" placeholder-style="color:#9a9a9a" maxlength="6" />
						<view class="ca-code" :class="{ off: smsLeft > 0 }" @click="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '获取验证码' }}</view>
					</view>
				</block>
				<block v-else>
					<view class="ca-line">
						<text class="ca-line__tag">邮箱</text>
						<input class="ca-line__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#9a9a9a" maxlength="40" />
					</view>
					<view class="ca-line">
						<text class="ca-line__tag">验证</text>
						<input class="ca-line__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#9a9a9a" maxlength="6" />
						<view class="ca-code" :class="{ off: mailLeft > 0 }" @click="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>
				</block>

				<view class="ca-btn" @click="submit"><text>打  卡</text></view>
				<view v-if="stamped" class="ca-count"><text>本月已打卡 {{ stampCount }} 天</text></view>

				<view class="ca-foot">
					<text>还没有账户？</text>
					<text class="ca-foot__link" @click="goRegister">立即报名</text>
				</view>

				<view class="ca-agree" @click="agreed = !agreed">
					<view class="ca-check" :class="{ on: agreed }"></view>
					<text>我已阅读并同意<text class="ca-link">《用户协议》</text>与<text class="ca-link">《隐私政策》</text></text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				seals: ["密码", "短信", "邮箱"],
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
				timer: null,
				week: ["日","一","二","三","四","五","六"],
				calYear: 2026,
				calMonth: 10,
				monthEn: 'OCTOBER',
				today: 1,
				calCells: [],
				stamped: false,
				stampCount: 0
			}
		},
		onLoad(options) {
			this.buildCalendar()
			// 演示参数：?tab=1&demo=1
			if (options && options.tab) this.tab = Number(options.tab) || 0
			if (options && options.demo) this.fillDemo()
		},
		onUnload() {
			this.clearTimer()
		},
		methods: {
			// 用系统日期生成当月月历数据（7 列网格，含首行空位）
			buildCalendar() {
				const now = new Date()
				this.calYear = now.getFullYear()
				this.calMonth = now.getMonth() + 1
				this.today = now.getDate()
				this.monthEn = ['JANUARY','FEBRUARY','MARCH','APRIL','MAY','JUNE','JULY','AUGUST','SEPTEMBER','OCTOBER','NOVEMBER','DECEMBER'][now.getMonth()]
				const days = new Date(this.calYear, this.calMonth, 0).getDate()
				const first = new Date(this.calYear, this.calMonth - 1, 1).getDay()
				const cells = []
				for (let i = 0; i < first; i++) cells.push({ n: 0, col: i })
				for (let d = 1; d <= days; d++) cells.push({ n: d, col: (first + d - 1) % 7 })
				this.calCells = cells
			},

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
				// 校验通过：今日格盖章 + 装饰计数
				this.stamped = true
				this.stampCount++
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goBack() {
				uni.navigateBack()
			},

			goRegister() {
				uni.navigateTo({ url: '/pages/login103/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login103/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
