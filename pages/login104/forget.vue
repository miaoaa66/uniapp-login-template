<template>
	<view class="bk-page">
		<!-- 顶部：积木块返回键 + 标题 -->
		<view class="bk-top">
			<view class="bk-back" @click="goBack">
				<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view></view>
				<view class="pxi pxi-back"></view>
			</view>
			<view class="bk-top__t">
				<text class="bk-top__cn">拆除重建</text>
				<text class="bk-top__en">REBUILD PASSWORD</text>
			</view>
		</view>

		<!-- 阶段指示：红积木（拆）→ 绿积木（建） -->
		<view class="bk-phase">
			<view class="bk-pbrick bk-pbrick--red" :class="{ on: step === 1 }">
				<view class="bk-pbrick__studs"><view class="bk-pstud"></view><view class="bk-pstud"></view></view>
			</view>
			<view class="pxi pxi-arrow bk-phase__arrow"></view>
			<view class="bk-pbrick bk-pbrick--green" :class="{ on: step === 2 }">
				<view class="bk-pbrick__studs"><view class="bk-pstud"></view><view class="bk-pstud"></view></view>
			</view>
			<text class="bk-phase__t">{{ step === 1 ? 'PHASE 1 TEAR' : 'PHASE 2 BUILD' }}</text>
		</view>

		<view :key="step">
			<block v-if="step === 1">
				<!-- 第一步：拆（验证邮箱） -->
				<view class="bk-brick bk-brick--orange" :class="{ up: focusField === 'email' }" style="animation-delay: .05s;">
					<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
					<input class="bk-brick__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#ffd9b3" maxlength="40" @focus="focusField = 'email'" @blur="focusField = ''" />
					<view class="bk-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
				</view>

				<view class="bk-cellbricks">
					<view v-for="(c, i) in cells" :key="i" class="bk-cellbrick" :class="{ on: focusIndex === i }">
						<view class="bk-cstuds"><view class="bk-cstud"></view><view class="bk-cstud"></view></view>
						<input class="bk-cellbrick__in" v-model="cells[i]" type="number" maxlength="1" placeholder-style="color:#aab2bd" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>
				</view>

				<view class="bk-next" @click="toStep2">
					<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
					<text>拆完了 · 开始重建</text>
				</view>
			</block>
			<block v-else>
				<!-- 第二步：建（重置密码） -->
				<view class="bk-brick bk-brick--yellow" :class="{ up: focusField === 'password' }" style="animation-delay: .05s;">
					<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
					<input class="bk-brick__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#8a7400" maxlength="20" @focus="focusField = 'password'" @blur="focusField = ''" />
					<view class="bk-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
				</view>
				<view class="bk-brick bk-brick--blue" :class="{ up: focusField === 'password2' }" style="animation-delay: .13s;">
					<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
					<input class="bk-brick__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#9fc4e8" maxlength="20" @focus="focusField = 'password2'" @blur="focusField = ''" />
				</view>

				<view class="bk-base" @click="submit">
					<view class="bk-studs"><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view><view class="bk-stud"></view></view>
					<text>REBUILD !</text>
				</view>

				<view v-if="rebuilt" class="bk-ok"><text>REBUILD OK</text></view>
			</block>
		</view>

		<view class="bk-foot">
			<text>想起密码了？</text>
			<text class="bk-foot__link" @click="goLogin">返回登录</text>
		</view>

		<view class="bk-floor"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				step: 1,
				rebuilt: false,
				showPwd: false,
				email: '',
				cells: ['', '', '', '', '', ''],
				focusIndex: 0,
				password: '',
				password2: '',
				mailLeft: 0,
				timer: null,
				focusField: ''
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
				this.rebuilt = true
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
