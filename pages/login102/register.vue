<template>
	<view class="ts-page">
		<view class="ts-device">
			<view class="ts-screw ts-screw--lt"></view>
			<view class="ts-screw ts-screw--rt"></view>
			<view class="ts-screw ts-screw--lb"></view>
			<view class="ts-screw ts-screw--rb"></view>

			<view class="ts-toprow">
				<view class="ts-back" @click="goBack">
					<view class="pxi pxi-back"></view>
				</view>
				<text class="ts-brandt">PIXELMAN · 注册</text>
				<view class="ts-led"></view>
			</view>

			<view class="ts-bay">
				<view class="ts-win">
					<view class="ts-reel ts-reel--l"></view>
					<view class="ts-eq">
						<view class="ts-eq__b ts-eq__b--0"></view>
						<view class="ts-eq__b ts-eq__b--1"></view>
						<view class="ts-eq__b ts-eq__b--2"></view>
						<view class="ts-eq__b ts-eq__b--3"></view>
						<view class="ts-eq__b ts-eq__b--4"></view>
					</view>
					<view class="ts-reel ts-reel--r"></view>
				</view>
				<view class="ts-winlabel">
					<text class="ts-winlabel__t">SIDE B · NEW ALBUM</text>
				</view>
			</view>

			<view class="ts-list">
				<view class="ts-track">
					<text class="ts-track__no">TRACK A1</text>
					<text class="ts-track__name">艺名</text>
					<input class="ts-track__in" v-model="nickname" placeholder="昵称（2-12 字）" placeholder-style="color:#8b939c" maxlength="12" />
				</view>

				<view class="ts-track">
					<text class="ts-track__no">TRACK A2</text>
					<text class="ts-track__name">联系方式</text>
					<input class="ts-track__in" v-model="email" placeholder="player@pixel.id" placeholder-style="color:#8b939c" maxlength="40" />
				</view>

				<view class="ts-track">
					<text class="ts-track__no">TRACK A3</text>
					<text class="ts-track__name">验证</text>
					<input class="ts-track__in" v-model="mailCode" type="number" placeholder="邮箱验证码" placeholder-style="color:#8b939c" maxlength="6" />
					<view class="ts-rec" :class="{ off: mailLeft > 0 }" @click="sendCode">
						<view class="ts-rec__dot"></view>
						<text class="ts-rec__t">{{ mailLeft > 0 ? mailLeft + 's' : 'REC' }}</text>
					</view>
				</view>

				<view class="ts-track">
					<text class="ts-track__no">TRACK A4</text>
					<text class="ts-track__name">主打歌</text>
					<input class="ts-track__in" v-model="password" :password="!showPwd" placeholder="设置密码（8-20 位）" placeholder-style="color:#8b939c" maxlength="20" />
					<view class="ts-hold" :class="{ on: showPwd }" @click="showPwd = !showPwd"><text class="ts-hold__t">HOLD</text></view>
				</view>

				<view class="ts-meter">
					<text class="ts-meter__t">LEVEL</text>
					<view class="ts-meter__b ts-meter__b--1" :class="{ on: meter >= 1 }"></view>
					<view class="ts-meter__b ts-meter__b--2" :class="{ on: meter >= 2 }"></view>
					<view class="ts-meter__b ts-meter__b--3" :class="{ on: meter >= 3 }"></view>
					<view class="ts-meter__b ts-meter__b--4" :class="{ on: meter >= 4 }"></view>
					<view class="ts-meter__b ts-meter__b--5" :class="{ on: meter >= 5 }"></view>
					<text class="ts-meter__v">{{ strengthText }}</text>
				</view>

				<view class="ts-track">
					<text class="ts-track__no">TRACK B1</text>
					<text class="ts-track__name">复刻</text>
					<input class="ts-track__in" v-model="password2" :password="!showPwd" placeholder="确认密码" placeholder-style="color:#8b939c" maxlength="20" />
				</view>

				<view class="ts-keyrow">
					<view class="ts-keycol">
						<view class="ts-agree" @click="agreed = !agreed">
							<view class="ts-check" :class="{ on: agreed }"></view>
							<text class="ts-agree__t">已阅读并同意<text class="ts-agree__lnk">《用户协议》</text>与<text class="ts-agree__lnk">《隐私政策》</text></text>
						</view>
						<view class="ts-foot">
							<text class="ts-foot__t">已有磁带？</text>
							<text class="ts-foot__link" @click="goLogin">换回 SIDE A → 返回登录</text>
						</view>
					</view>
					<view class="ts-play" @click="submit">
						<text class="ts-play__t">REC ▶</text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
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
			// strength 0-3 映射为 5 根电平柱
			meter() {
				return [0, 2, 3, 5][this.strength]
			},
			strengthText() {
				return ['LOW', 'LOW', 'MID', 'HIGH'][this.strength]
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

			submit() {
				if (!this.nickname) return this.toast('请输入昵称')
				if (!this.isEmail(this.email)) return this.toast('请输入正确的邮箱')
				if (!this.mailCode) return this.toast('请输入邮箱验证码')
				if (this.password.length < 8) return this.toast('密码至少 8 位')
				if (this.password !== this.password2) return this.toast('两次输入的密码不一致')
				if (!this.agreed) return this.toast('请先阅读并同意用户协议')
				// TODO: 调用后端注册接口
				this.toast('注册成功')
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
