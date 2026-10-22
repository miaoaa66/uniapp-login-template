<template>
	<view class="ca-page">
		<!-- 翻页台历：迷你月历显示页码与「已重置」章 -->
		<view class="ca-cal ca-cal--mini">
			<view class="ca-cal__rings"><view class="ca-ring"></view><view class="ca-ring"></view></view>
			<view class="ca-cal__head">
				<view class="ca-cal__back" @click="goBack"><view class="pxi pxi-back"></view></view>
				<view class="ca-cal__title">
					<text class="ca-cal__cn">{{ calMonth }}月 {{ today }} 日</text>
					<text class="ca-cal__en">{{ monthEn }}</text>
				</view>
				<text class="ca-cal__today">{{ step }}/2</text>
			</view>
			<view class="ca-cal__week">
				<text v-for="(w, i) in week" :key="i" class="ca-cal__wd" :class="{ red: i === 0 || i === 6 }">{{ w }}</text>
			</view>
			<view class="ca-cal__grid">
				<view v-for="(c, i) in calCells" :key="i" class="ca-cal__cell" :class="{ red: c.col === 0 || c.col === 6, today: c.n === today }">
					<text v-if="c.n" class="ca-cal__num">{{ c.n }}</text>
					<view v-if="c.n === today && resetStamped" class="ca-stamp ca-stamp--txt"><text class="ca-stamp__t">已重置</text></view>
				</view>
			</view>
		</view>

		<!-- 撕页面板 -->
		<view class="ca-panel">
			<view class="ca-panel__tear"></view>
			<text class="ca-panel__t">翻页找回</text>
			<text class="ca-panel__sub">一天一页，撕到密码回来为止</text>

			<view class="ca-flip" :key="step" :class="{ out: tearing }">
				<block v-if="step === 1">
					<view class="ca-form">
						<view class="ca-line">
							<text class="ca-line__tag">邮箱</text>
							<input class="ca-line__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#9a9a9a" maxlength="40" />
							<view class="ca-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
						</view>
						<text class="ca-form__cap">输入 6 位日期格验证码</text>
						<view class="ca-datecells">
							<input v-for="(c, i) in cells" :key="i" class="ca-date" :class="{ dim: !c, on: focusIndex === i }"
								v-model="cells[i]" type="number" maxlength="1" placeholder-style="color:#d43c33" :focus="focusIndex === i" @input="onCell(i, $event)" />
						</view>
						<view class="ca-btn" @click="toStep2"><text>撕到下一页</text></view>
					</view>
				</block>
				<block v-else>
					<view class="ca-form">
						<view class="ca-line">
							<text class="ca-line__tag">新密码</text>
							<input class="ca-line__in" v-model="password" :password="!showPwd" placeholder="8-20 位" placeholder-style="color:#9a9a9a" maxlength="20" />
							<view class="ca-line__eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
						<view class="ca-line">
							<text class="ca-line__tag">确认</text>
							<input class="ca-line__in" v-model="password2" :password="!showPwd" placeholder="再输一遍" placeholder-style="color:#9a9a9a" maxlength="20" />
						</view>
						<view class="ca-btn" @click="submit"><text>盖上「已重置」章</text></view>
					</view>
				</block>
			</view>

			<view class="ca-foot">
				<text>想起密码了？</text>
				<text class="ca-foot__link" @click="goLogin">返回登录</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				step: 1,
				tearing: false,
				resetStamped: false,
				showPwd: false,
				email: '',
				cells: ['', '', '', '', '', ''],
				focusIndex: 0,
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null,
				week: ["日","一","二","三","四","五","六"],
				calMonth: 10,
				monthEn: 'OCTOBER',
				today: 1,
				calCells: []
			}
		},
		onLoad(options) {
			this.buildCalendar()
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
			buildCalendar() {
				const now = new Date()
				this.calMonth = now.getMonth() + 1
				this.today = now.getDate()
				this.monthEn = ['JANUARY','FEBRUARY','MARCH','APRIL','MAY','JUNE','JULY','AUGUST','SEPTEMBER','OCTOBER','NOVEMBER','DECEMBER'][now.getMonth()]
				const days = new Date(now.getFullYear(), this.calMonth, 0).getDate()
				const first = new Date(now.getFullYear(), this.calMonth - 1, 1).getDay()
				const cells = []
				for (let i = 0; i < first; i++) cells.push({ n: 0, col: i })
				for (let d = 1; d <= days; d++) cells.push({ n: d, col: (first + d - 1) % 7 })
				this.calCells = cells
			},

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

			// 撕页动画：旧页滑出后切到第二步
			toStep2() {
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
				this.tearing = true
				setTimeout(() => {
					this.step = 2
					this.tearing = false
				}, 300)
			},

			submit() {
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
				if (this.password.length < 8) return this.toast('新密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				this.resetStamped = true
				// TODO: 调用后端重置密码接口
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
