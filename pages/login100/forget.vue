<template>
	<view class="sp-page">
		<view class="sp-dome sp-dome--slim">
			<view class="sp-exit" @click="goBack">
				<text class="sp-exit__t">EXIT ▸</text>
			</view>
			<view class="sp-tick sp-tick--lt"></view>
			<view class="sp-tick sp-tick--rt"></view>
			<view class="sp-tick sp-tick--lb"></view>
			<view class="sp-tick sp-tick--rb"></view>
			<view class="sp-radar">
				<view class="sp-radar__ring sp-radar__ring--out"></view>
				<view class="sp-radar__ring sp-radar__ring--mid"></view>
				<view class="sp-radar__ring sp-radar__ring--in"></view>
				<view class="sp-radar__cross"></view>
				<view class="sp-radar__sweep"></view>
				<view class="sp-radar__blip sp-radar__blip--a"></view>
				<view class="sp-radar__blip sp-radar__blip--b"></view>
			</view>
			<text class="sp-dome__cap">RECOVERY PROTOCOL · 恢复协议</text>
		</view>

		<view class="sp-console">
			<view class="sp-phase">
				<view class="sp-phase__row" :class="{ on: step >= 1 }">
					<view class="sp-phase__dot"></view>
					<text class="sp-phase__t">PHASE 1 ▸ 邮箱核验</text>
				</view>
				<view class="sp-phase__row" :class="{ on: step >= 2 }">
					<view class="sp-phase__dot"></view>
					<text class="sp-phase__t">PHASE 2 ▸ 密钥重写</text>
				</view>
			</view>

			<view class="sp-form">
				<view class="sp-line" :class="{ lock: focusKey === 'email' }">
					<text class="sp-line__k">COMMS ▸</text>
					<input class="sp-line__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#3d5a52" maxlength="40" @focus="focusKey = 'email'" @blur="focusKey = ''" />
					<view class="sp-send" :class="{ off: mailLeft > 0 }" @click="sendCode"><text class="sp-send__t">{{ mailLeft > 0 ? mailLeft + 's' : 'SEND ▸' }}</text></view>
					<view class="sp-line__c sp-line__c--tr"></view>
					<view class="sp-line__c sp-line__c--bl"></view>
				</view>

				<view class="sp-cells">
					<input v-for="(c, i) in cells" :key="i" class="sp-cell" :class="{ dim: !c, on: focusIndex === i }"
						v-model="cells[i]" type="number" maxlength="1" placeholder-style="color:#5a6b7a" :focus="focusIndex === i" @input="onCell(i, $event)" />
				</view>
				<text class="sp-cells__cap">COORD READOUT · 6 位坐标读数</text>

				<view class="sp-line" :class="{ lock: focusKey === 'password' }">
					<text class="sp-line__k">NEWKEY ▸</text>
					<input class="sp-line__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#3d5a52" maxlength="20" @focus="focusKey = 'password'" @blur="focusKey = ''" />
					<view class="sp-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					<view class="sp-line__c sp-line__c--tr"></view>
					<view class="sp-line__c sp-line__c--bl"></view>
				</view>

				<view class="sp-line" :class="{ lock: focusKey === 'password2' }">
					<text class="sp-line__k">CONFIRM ▸</text>
					<input class="sp-line__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#3d5a52" maxlength="20" @focus="focusKey = 'password2'" @blur="focusKey = ''" />
					<view class="sp-line__c sp-line__c--tr"></view>
					<view class="sp-line__c sp-line__c--bl"></view>
				</view>

				<view class="sp-engage" :class="{ charging: charging }" @click="submit">
					<view class="sp-engage__charge"></view>
					<text class="sp-engage__t">RESTORE ▸ 恢复密钥</text>
				</view>

				<view class="sp-foot">
					<text class="sp-foot__t">想起密码了？</text>
					<text class="sp-foot__link" @click="goLogin">RELINK ▸ 返回登录</text>
				</view>

				<view v-if="okFlash" class="sp-flash"><text class="sp-flash__t">SIGNAL RESTORED · 信号已恢复</text></view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				step: 1,
				showPwd: false,
				email: '',
				cells: ['', '', '', '', '', ''],
				focusIndex: 0,
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null,
				focusKey: '',
				charging: false,
				okFlash: false
			}
		},
		computed: {
			codeFull() {
				return this.cells.join('').length >= 6
			}
		},
		watch: {
			codeFull(v) {
				if (v && this.step < 2) this.step = 2
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

			// 按下后充电条 0.6s 充满再执行校验
			submit() {
				if (this.charging) return
				this.charging = true
				setTimeout(() => {
					this.charging = false
					this.doSubmit()
				}, 600)
			},

			doSubmit() {
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
				if (this.password.length < 8) return this.toast('新密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				// TODO: 调用后端重置密码接口
				this.toast('密码已重置')
				this.okFlash = true
				setTimeout(() => { this.okFlash = false }, 2200)
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
