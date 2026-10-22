<template>
	<view class="rq-page">
		<!-- 场景带（神殿剪影） -->
		<view class="rq-scene rq-scene--short">
			<view class="rq-back" @click="goBack"><text class="rq-back__t">‹ 返回</text></view>
			<view class="rq-pillar rq-pillar--a"></view>
			<view class="rq-pillar rq-pillar--b"></view>
			<view class="rq-pillar rq-pillar--c"></view>
			<view class="rq-ground"></view>
		</view>

		<!-- RPG 对话框：神殿重铸 -->
		<view class="rq-dialog">
			<view class="rq-dialog__deco"></view>
			<view class="rq-dialog__head">
				<text class="rq-dialog__npc">长老 ORIN · 神殿重铸</text>
			</view>
			<text class="rq-dialog__text">{{ stepLines[step - 1] }}</text>

			<!-- 两块石碑步骤 -->
			<view class="rq-tablets">
				<view class="rq-tablet" :class="{ on: step === 1 }"><text>一 · 信标</text></view>
				<view class="rq-tablet" :class="{ on: step === 2 }"><text>二 · 重铸</text></view>
			</view>

			<view class="rq-form">
				<block v-if="step === 1">
					<view class="rq-slot">
						<view class="rq-slot__ico"><view class="pxi pxi-mail"></view></view>
						<input class="rq-slot__in" v-model="email" placeholder="注册时的信箱" placeholder-style="color:#a08e6d" maxlength="40" />
						<view class="rq-scroll" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '赐下印文' }}</view>
					</view>
					<!-- 六格石板验证码 -->
					<view class="rq-cells">
						<input v-for="(c, i) in cells" :key="i" class="rq-cell" :class="{ dim: !c }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>
				</block>
				<block v-else>
					<view class="rq-slot">
						<view class="rq-slot__ico"><view class="pxi pxi-lock"></view></view>
						<input class="rq-slot__in" v-model="password" :password="!showPwd" placeholder="新咒语（8-20 位）" placeholder-style="color:#a08e6d" maxlength="20" />
						<view class="rq-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>
					<view class="rq-slot">
						<view class="rq-slot__ico"><view class="pxi pxi-lock"></view></view>
						<input class="rq-slot__in" v-model="password2" :password="!showPwd" placeholder="复述新咒语" placeholder-style="color:#a08e6d" maxlength="20" />
					</view>
				</block>
			</view>

			<view class="rq-foot">
				<text class="rq-foot__tip">{{ step === 1 ? '刻下 6 位印文' : '重铸你的咒语' }}</text>
				<view class="rq-go" @click="next"><text class="rq-go__t">▶</text></view>
			</view>
		</view>

		<view class="rq-pagefoot">
			<text>想起咒语了？</text>
			<text class="rq-pagefoot__link" @click="goLogin">返回登录</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				step: 1,
				stepLines: [
					'长老：报上你的信箱坐标，神殿将赐下六格刻印。',
					'长老：刻印无误。现在重铸你的咒语吧，旅行者。'
				],
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

			next() {
				if (this.step === 1) {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
					this.step = 2
				} else {
					if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
					if (this.cells.join('').length < 6) return this.toast('请输入 6 位验证码')
					if (this.password.length < 8) return this.toast('新密码至少 8 位')
					if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
					// TODO: 调用后端重置密码接口
					this.toast('密码已重置')
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
