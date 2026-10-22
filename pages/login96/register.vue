<template>
	<view class="rq-page">
		<!-- 场景带（短版） -->
		<view class="rq-scene rq-scene--short">
			<view class="rq-back" @click="goBack"><text class="rq-back__t">‹ 返回</text></view>
			<view class="rq-sun"></view>
			<view class="rq-mtn rq-mtn--a"></view>
			<view class="rq-tree rq-tree--a"><view class="rq-tree__crown"></view><view class="rq-tree__trunk"></view></view>
			<view class="rq-ground"></view>
		</view>

		<!-- RPG 对话框：新手任务 -->
		<view class="rq-dialog">
			<view class="rq-dialog__deco"></view>
			<view class="rq-dialog__head">
				<text class="rq-dialog__npc">村长 ROEL · 新手任务</text>
			</view>
			<text class="rq-dialog__text">{{ questLines[questStep - 1] }}</text>

			<!-- 步骤指示器：3 个宝箱 -->
			<view class="rq-chests">
				<view v-for="n in 3" :key="n" class="rq-chest" :class="{ done: questStep > n, now: questStep === n }">
					<view class="rq-chest__lid"></view>
					<view class="rq-chest__body"></view>
					<text class="rq-chest__mark" v-if="questStep > n">✓</text>
					<text class="rq-chest__label">{{ chestNames[n - 1] }}</text>
				</view>
			</view>

			<!-- 当前步字段 -->
			<view class="rq-form">
				<block v-if="questStep === 1">
					<view class="rq-slot">
						<view class="rq-slot__ico"><view class="pxi pxi-user"></view></view>
						<input class="rq-slot__in" v-model="nickname" placeholder="角色名（2-12 字）" placeholder-style="color:#a08e6d" maxlength="12" />
					</view>
				</block>
				<block v-else-if="questStep === 2">
					<view class="rq-slot">
						<view class="rq-slot__ico"><view class="pxi pxi-mail"></view></view>
						<input class="rq-slot__in" v-model="email" placeholder="信箱：player@pixel.id" placeholder-style="color:#a08e6d" maxlength="40" />
					</view>
					<view class="rq-slot">
						<view class="rq-slot__ico"><view class="pxi pxi-shield"></view></view>
						<input class="rq-slot__in" v-model="mailCode" type="number" placeholder="契约印文" placeholder-style="color:#a08e6d" maxlength="6" />
						<view class="rq-scroll" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '领取印文' }}</view>
					</view>
				</block>
				<block v-else>
					<view class="rq-slot">
						<view class="rq-slot__ico"><view class="pxi pxi-lock"></view></view>
						<input class="rq-slot__in" v-model="password" :password="!showPwd" placeholder="咒语（8-20 位）" placeholder-style="color:#a08e6d" maxlength="20" />
						<view class="rq-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>
					<view class="rq-slot">
						<view class="rq-slot__ico"><view class="pxi pxi-lock"></view></view>
						<input class="rq-slot__in" v-model="password2" :password="!showPwd" placeholder="复述咒语" placeholder-style="color:#a08e6d" maxlength="20" />
					</view>
					<view class="rq-power">
						<text class="rq-power__t">咒语强度</text>
						<view class="rq-power__bars">
							<view class="rq-power__bar" :class="{ on: strength >= 1 }"></view>
							<view class="rq-power__bar" :class="{ on: strength >= 2 }"></view>
							<view class="rq-power__bar" :class="{ on: strength >= 3 }"></view>
						</view>
						<text class="rq-power__v">{{ strengthText }}</text>
					</view>
					<view class="rq-agree" @click="agreed = !agreed">
						<view class="rq-seal" :class="{ on: agreed }"></view>
						<text class="rq-agree__t">立约 · 已读<text class="rq-agree__link">《用户协议》</text>与<text class="rq-agree__link">《隐私政策》</text></text>
					</view>
				</block>
			</view>

			<!-- 右下角继续键 -->
			<view class="rq-foot">
				<text class="rq-foot__tip">完成本步后按 ▶ 继续</text>
				<view class="rq-go" @click="nextStep"><text class="rq-go__t">▶</text></view>
			</view>
		</view>

		<view class="rq-pagefoot">
			<text>已有通行证？</text>
			<text class="rq-pagefoot__link" @click="goLogin">去登录</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				questStep: 1,
				chestNames: ['角色', '契约', '咒语'],
				questLines: [
					'建立角色：为你的冒险者取一个响亮的名字吧。',
					'立下契约：留下信箱坐标，领取契约印文。',
					'设置咒语：念出一段只有你知道的咒语，并再次复述。'
				],
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
			strengthText() {
				return ['──', '弱', '中', '强'][this.strength]
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

			// 每步末尾执行该步校验，第 3 步执行完整校验（覆盖全部字段）
			nextStep() {
				if (this.questStep === 1) {
					if (!this.nickname) return this.toast('请输入昵称')
					this.questStep = 2
				} else if (this.questStep === 2) {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (!this.mailCode) return this.toast('请输入邮箱验证码')
					this.questStep = 3
				} else {
					if (this.password.length < 8) return this.toast('密码至少 8 位')
					if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
					if (!this.agreed) return this.toast('请先阅读并同意用户协议')
					// TODO: 调用后端注册接口
					this.toast('任务完成!')
				}
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
