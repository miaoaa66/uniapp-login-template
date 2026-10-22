<template>
	<view class="vd-page">
		<!-- 机身：顶盖灯箱 + 橱窗 + 服务面板 -->
		<view class="vd-machine">
			<view class="vd-cap">
				<view class="vd-cap__back" @click="goBack"><view class="pxi pxi-back"></view></view>
				<view class="vd-sign">
					<view class="vd-sign__lamp"></view>
					<text class="vd-sign__t">服务中心 · SERVICE</text>
					<view class="vd-sign__lamp vd-sign__lamp--r"></view>
				</view>
			</view>

			<!-- 橱窗（装饰货道） -->
			<view class="vd-window">
				<view class="vd-window__tube"></view>
				<view class="vd-slots">
					<view v-for="(s, i) in slots" :key="i" class="vd-slot">
						<view class="vd-bag" :class="'vd-bag--' + i">
							<view class="vd-bag__seal"></view>
							<view class="vd-bag__ico"><view class="pxi" :class="s.icon"></view></view>
						</view>
						<text class="vd-slot__name">{{ s.name }}</text>
						<text class="vd-slot__no">{{ s.no }}</text>
					</view>
				</view>
			</view>

			<view class="vd-panel">
				<block v-if="!serviceMode">
					<text class="vd-panel__title">吞币了？</text>
					<text class="vd-panel__sub">按下黄色按钮，值班补货员马上到场</text>
					<view class="vd-call" :class="{ blink: calling }" @click="callService"><text>服务呼叫 SERVICE</text></view>
				</block>
				<block v-else>
					<text class="vd-servtag">SERVICE MODE {{ step === 1 ? '1/2 MAIL' : '2/2 RESET' }}</text>
					<view :key="step">
						<block v-if="step === 1">
							<view class="vd-line">
								<text class="vd-line__tag">邮箱</text>
								<input class="vd-line__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#8b97a8" maxlength="40" />
								<view class="vd-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
							</view>
							<text class="vd-form__cap">投入 6 位验证码</text>
							<view class="vd-coincells">
								<input v-for="(c, i) in cells" :key="i" class="vd-coincell" :class="{ on: focusIndex === i }"
									v-model="cells[i]" type="number" maxlength="1" placeholder-style="color:#2a3a55" :focus="focusIndex === i" @input="onCell(i, $event)" />
							</view>
							<view class="vd-btn" @click="toStep2"><text>下一步</text></view>
						</block>
						<block v-else>
							<view class="vd-line">
								<text class="vd-line__tag">新密码</text>
								<input class="vd-line__in" v-model="password" :password="!showPwd" placeholder="8-20 位" placeholder-style="color:#8b97a8" maxlength="20" />
								<view class="vd-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
							</view>
							<view class="vd-line">
								<text class="vd-line__tag">确认</text>
								<input class="vd-line__in" v-model="password2" :password="!showPwd" placeholder="再输一遍" placeholder-style="color:#8b97a8" maxlength="20" />
							</view>
							<view class="vd-btn" @click="submit"><text>服 务 完 成</text></view>
						</block>
					</view>
				</block>

				<view class="vd-foot">
					<text>想起密码了？</text>
					<text class="vd-foot__link" @click="goLogin">返回登录</text>
				</view>
			</view>

			<view class="vd-hatch"><view class="vd-hatch__door"><text>PUSH</text></view></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				slots: [
					{ no: 'A1', name: '密码登录', icon: 'pxi-lock' },
					{ no: 'A2', name: '短信验证', icon: 'pxi-phone' },
					{ no: 'A3', name: '邮箱验证', icon: 'pxi-mail' }
				],
				serviceMode: false,
				calling: false,
				step: 1,
				showPwd: false,
				email: '',
				cells: ['', '', '', '', '', ''],
				focusIndex: 0,
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null
			}
		},
		onLoad(options) {
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
			// 按下服务呼叫：按钮闪烁半秒后切入服务模式
			callService() {
				if (this.calling) return
				this.calling = true
				setTimeout(() => {
					this.serviceMode = true
					this.calling = false
				}, 500)
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

			toStep2() {
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
				this.step = 2
			},

			submit() {
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
				if (this.password.length < 8) return this.toast('新密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
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
