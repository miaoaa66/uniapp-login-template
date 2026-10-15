<template>
	<view class="rq-page">
		<view class="rq-nav">
			<view class="rq-nav__back" @click="goBack">
				<view class="pxi pxi-back"></view>
			</view>
			<text class="rq-nav__title">找回密码</text>
			<text class="rq-nav__slot">RETRY</text>
		</view>

		<view class="rq-body">
			<view class="rq-status">
				<view class="rq-face"><view class="pxi pxi-invader"></view></view>
				<view class="rq-status__meta">
					<text class="rq-status__name">HERO  LV.12  ·  冒险者</text>
					<view class="rq-bar rq-bar--hp">
						<view class="rq-bar__fill" style="width: 78%"></view>
						<text class="rq-bar__t">HP 78/100</text>
					</view>
					<view class="rq-bar rq-bar--mp">
						<view class="rq-bar__fill" style="width: 46%"></view>
						<text class="rq-bar__t">MP 46/100</text>
					</view>
				</view>
			</view>
			<view class="rq-card">
				<view class="rq-brand">
					<view class="rq-brand__logo"><view class="pxi pxi-key"></view></view>
					<view class="rq-brand__meta">
						<text class="rq-brand__en">REVIVE</text>
						<text class="rq-brand__cn">复活咒语</text>
					</view>
				</view>

				<text class="rq-title">找回密码</text>
				<text class="rq-lead">吟唱复活咒语，重设你的密码</text>

				<view class="rq-steps">
					<view class="rq-step" :class="{ on: step >= 1 }">
						<view class="rq-step__n">1</view>
						<text>验证邮箱</text>
					</view>
					<view class="rq-step__line" :class="{ on: step >= 2 }"></view>
					<view class="rq-step" :class="{ on: step >= 2 }">
						<view class="rq-step__n">2</view>
						<text>重置密码</text>
					</view>
				</view>

				<view class="rq-form">
					<view class="rq-field">
						<view class="rq-field__ico"><view class="pxi pxi-mail"></view></view>
						<input class="rq-field__in" v-model="email" placeholder="注册时使用的邮箱" placeholder-style="color:#a08e6d" maxlength="40" />
						<view class="rq-code" :class="{ off: mailLeft > 0 }" @click="sendCode">{{ mailLeft > 0 ? mailLeft + 's' : '获取验证码' }}</view>
					</view>

					<view class="rq-cells">
						<input v-for="(c, i) in cells" :key="i" class="rq-cell" :class="{ dim: !c, on: focusIndex === i }"
							v-model="cells[i]" type="number" maxlength="1" :focus="focusIndex === i" @input="onCell(i, $event)" />
					</view>

					<view class="rq-field">
						<view class="rq-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="rq-field__in" v-model="password" :password="!showPwd" placeholder="新密码（8-20 位）" placeholder-style="color:#a08e6d" maxlength="20" />
						<view class="rq-eye" @click="showPwd = !showPwd"><view class="pxi pxi-eye"></view></view>
					</view>

					<view class="rq-field">
						<view class="rq-field__ico"><view class="pxi pxi-lock"></view></view>
						<input class="rq-field__in" v-model="password2" :password="!showPwd" placeholder="确认新密码" placeholder-style="color:#a08e6d" maxlength="20" />
					</view>

					<view class="rq-btn" @click="submit">
						<view class="pxi pxi-play"></view><text>复活存档</text>
					</view>

					<view class="rq-foot">
						<text>想起密码了？</text>
						<text class="rq-foot__link" @click="goLogin">返回登录</text>
					</view>
				</view>
			</view>
			<view class="rq-quest">
				<view class="rq-quest__box">
					<text class="rq-quest__t">在告示板上签下你的名字，冒险即将开始</text>
					<view class="rq-quest__arrow"></view>
				</view>
				<view class="rq-chest">
					<view class="rq-chest__lid"></view>
					<view class="rq-chest__body"></view>
					<view class="rq-chest__lock"></view>
				</view>
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
