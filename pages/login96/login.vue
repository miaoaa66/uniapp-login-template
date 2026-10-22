<template>
	<view class="rq-page">
		<!-- 上部：场景带（羊皮纸 + 远山/树/旗/太阳） -->
		<view class="rq-scene">
			<view class="rq-back" @click="goBack"><text class="rq-back__t">‹ 返回</text></view>
			<view class="rq-sun"></view>
			<view class="rq-mtn rq-mtn--a"></view>
			<view class="rq-mtn rq-mtn--b"></view>
			<view class="rq-tree rq-tree--a"><view class="rq-tree__crown"></view><view class="rq-tree__trunk"></view></view>
			<view class="rq-tree rq-tree--b"><view class="rq-tree__crown"></view><view class="rq-tree__trunk"></view></view>
			<view class="rq-flag"><view class="rq-flag__pole"></view><view class="rq-flag__cloth"></view></view>
			<view class="rq-ground"></view>
		</view>

		<!-- 下部：RPG 对话框 -->
		<view class="rq-dialog" @click="skipType">
			<view class="rq-dialog__deco"></view>
			<view class="rq-dialog__head">
				<text class="rq-dialog__npc">村长 ROEL</text>
				<text class="rq-dialog__hint" v-if="typing">点击跳过 ▸</text>
			</view>
			<text class="rq-dialog__text">{{ shown }}<text v-if="typing" class="rq-caret">▌</text></text>

			<block v-if="typed">
				<!-- 对话选项 = 登录方式切换 -->
				<view class="rq-opts">
					<view v-for="(item, i) in tabs" :key="i" class="rq-opt" :class="{ on: tab === i }" @click.stop="tab = i">
						<text class="rq-opt__arrow">▶</text>
						<text class="rq-opt__t">{{ item }}</text>
					</view>
				</view>

				<!-- 道具栏表单 -->
				<view class="rq-form">
					<block v-if="tab === 0">
						<view class="rq-slot">
							<view class="rq-slot__ico"><view class="pxi pxi-user"></view></view>
							<input class="rq-slot__in" v-model="account" placeholder="通行证：手机号 / 邮箱" placeholder-style="color:#a08e6d" maxlength="40" />
						</view>
						<view class="rq-slot">
							<view class="rq-slot__ico"><view class="pxi pxi-lock"></view></view>
							<input class="rq-slot__in" v-model="password" :password="!showPwd" placeholder="咒语：登录密码" placeholder-style="color:#a08e6d" maxlength="20" />
							<view class="rq-eye" @click.stop="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
						</view>
						<view class="rq-aux"><text class="rq-aux__link" @click.stop="goForget">忘记咒语了？</text></view>
					</block>
					<block v-else-if="tab === 1">
						<view class="rq-slot">
							<view class="rq-slot__ico"><view class="pxi pxi-phone"></view></view>
							<input class="rq-slot__in rq-slot__in--en" v-model="phone" type="number" placeholder="+86" placeholder-style="color:#a08e6d" maxlength="11" />
						</view>
						<view class="rq-slot">
							<view class="rq-slot__ico"><view class="pxi pxi-shield"></view></view>
							<input class="rq-slot__in" v-model="smsCode" type="number" placeholder="短信印文" placeholder-style="color:#a08e6d" maxlength="6" />
							<view class="rq-scroll" :class="{ off: smsLeft > 0 }" @click.stop="sendCode('sms')">{{ smsLeft > 0 ? smsLeft + 's' : '领取印文' }}</view>
						</view>
					</block>
					<block v-else>
						<view class="rq-slot">
							<view class="rq-slot__ico"><view class="pxi pxi-mail"></view></view>
							<input class="rq-slot__in" v-model="email" placeholder="信箱：player@pixel.id" placeholder-style="color:#a08e6d" maxlength="40" />
						</view>
						<view class="rq-slot">
							<view class="rq-slot__ico"><view class="pxi pxi-shield"></view></view>
							<input class="rq-slot__in" v-model="mailCode" type="number" placeholder="信鹰印文" placeholder-style="color:#a08e6d" maxlength="6" />
							<view class="rq-scroll" :class="{ off: mailLeft > 0 }" @click.stop="sendCode('mail')">{{ mailLeft > 0 ? mailLeft + 's' : '领取印文' }}</view>
						</view>
					</block>
				</view>

				<!-- 立约 + 对话框右下角大 ▶ 键 -->
				<view class="rq-foot">
					<view class="rq-agree" @click.stop="agreed = !agreed">
						<view class="rq-seal" :class="{ on: agreed }"></view>
						<text class="rq-agree__t">立约 · 已读<text class="rq-agree__link">《用户协议》</text>与<text class="rq-agree__link">《隐私政策》</text></text>
					</view>
					<view class="rq-go" @click.stop="submit"><text class="rq-go__t">▶</text></view>
				</view>
			</block>
		</view>

		<view class="rq-pagefoot">
			<text>还没有通行证？</text>
			<text class="rq-pagefoot__link" @click="goRegister">去刻一枚</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabs: ["使用密码", "使用短信验证", "使用邮箱验证"],
				tab: 0,
				line: '旅行者，请出示你的通行证……选择你想进入村庄的方式。',
				shown: '',
				typing: false,
				typed: false,
				typeTimer: null,
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
			if (options && (options.tab || options.demo)) {
				// 指定模式 / 演示模式时跳过打字机
				this.finishType()
			} else {
				this.startType()
			}
		},
		onUnload() {
			this.clearType()
			this.clearTimer()
		},
		methods: {
			startType() {
				this.clearType()
				this.shown = ''
				this.typing = true
				this.typeTimer = setInterval(() => {
					if (this.shown.length >= this.line.length) return this.finishType()
					this.shown = this.line.slice(0, this.shown.length + 1)
				}, 65)
			},

			finishType() {
				this.clearType()
				this.shown = this.line
				this.typing = false
				this.typed = true
			},

			skipType() {
				if (this.typing) this.finishType()
			},

			clearType() {
				if (this.typeTimer) {
					clearInterval(this.typeTimer)
					this.typeTimer = null
				}
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
				// TODO: 调用后端登录接口，成功后跳转首页
				this.toast('登录成功')
			},

			goBack() {
				uni.navigateBack()
			},

			goRegister() {
				uni.navigateTo({ url: '/pages/login96/register' })
			},

			goForget() {
				uni.navigateTo({ url: '/pages/login96/forget' })
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './style.scss';
</style>
