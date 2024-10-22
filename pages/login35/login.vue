<template>
	<view class="content">
		<view class="logo-box">
			<image class="logo" mode="aspectFill"
				src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login35/login35_logo.png">
			</image>
		</view>
		<view class="tab-box">
			<view :class="[isLogin ? 'active' : '','tab-item','login-half']" @click="switchClick(1)">登录</view>
			<view :class="[isLogin ? '' : 'active','tab-item','register-half']" @click="switchClick(2)">注册</view>
		</view>
		<view class="form-box">
			<view class="row">
				<input maxlength="11" placeholder="请输入手机号" type="number" />
			</view>
			<view class="row">
				<input maxlength="18" placeholder="请输入密码" :password="!password" />
				<image mode="aspectFill" v-if="password" @click="passwordClick"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login35/login35_password.png">
				</image>
				<image mode="aspectFill" v-else @click="passwordClick"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login35/login35_hide.png">
				</image>
			</view>
			<view class="row" v-if="!isLogin">
				<input maxlength="18" placeholder="请确认密码" :password="!confirmPassword" />
				<image mode="aspectFill" v-if="confirmPassword" @click="confirmPasswordClick"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login35/login35_password.png">
				</image>
				<image mode="aspectFill" v-else @click="confirmPasswordClick"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login35/login35_hide.png">
				</image>
			</view>
			<view class="code">
				<input maxlength="6" placeholder="验证码" type="number" />
				<view class="send-code">发送验证码</view>
			</view>
		</view>
		<view class="login-btn">立即登录</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isLogin: true,
				password: true,
				confirmPassword: true
			}
		},
		onLoad() {

		},
		methods: {

			// 切换
			switchClick(type) {
				this.isLogin = type == 1 ? true : false
			},

			passwordClick() {
				this.password = !this.password
			},

			confirmPasswordClick() {
				this.confirmPassword = !this.confirmPassword
			}

		}
	}
</script>

<style lang="scss">
	page {
		background-color: #d3e5fe;
	}

	.content {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;

		.logo-box {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 400rpx;

			.logo {
				width: 200rpx;
				height: 200rpx;
			}
		}

		.tab-box {
			display: flex;
			justify-content: center;
			align-items: center;

			.tab-item {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 260rpx;
				height: 90rpx;
				box-shadow: 5rpx 5rpx 4rpx #545c66,
					-5rpx -5rpx 4rpx #ffffff;
			}

			.login-half {
				border-top-left-radius: 50rpx;
				border-bottom-left-radius: 50rpx;
			}

			.register-half {
				border-top-right-radius: 50rpx;
				border-bottom-right-radius: 50rpx;
			}

			.active {
				box-shadow: inset 5rpx 5rpx 4rpx #545c66,
					inset -5rpx -5rpx 4rpx #ffffff;
				color: #9c9c9c;
			}
		}


		.form-box {
			padding-top: 40rpx;
			display: flex;
			flex-direction: column;
			align-items: center;

			.row {
				margin-top: 40rpx;
				display: flex;
				justify-content: flex-start;
				align-items: center;
				font-size: 30rpx;
				width: 660rpx;
				height: 105rpx;
				box-shadow: inset 5rpx 5rpx 4rpx #545c66,
					inset -5rpx -5rpx 4rpx #ffffff;
				border-radius: 100rpx;

				input {
					padding: 0 30rpx;
					width: 78%;
					height: 100%;
					color: #9c9c9c;
				}

				image {
					width: 40rpx;
					height: 40rpx;
					flex-shrink: 0;
				}
			}

			.code {
				margin-top: 40rpx;
				display: flex;
				justify-content: space-between;
				align-items: center;
				width: 660rpx;
				height: 105rpx;

				input {
					width: 50%;
					height: 100%;
					padding: 0 30rpx;
					box-shadow: inset 5rpx 5rpx 4rpx #545c66,
						inset -5rpx -5rpx 4rpx #ffffff;
					border-radius: 50rpx;
				}

				.send-code {
					display: flex;
					justify-content: center;
					align-items: center;
					font-size: 30rpx;
					width: 35%;
					height: 100%;
					box-shadow: 5rpx 5rpx 4rpx #545c66,
						-5rpx -5rpx 4rpx #ffffff;
					border-radius: 50rpx;
				}
			}
		}

		.login-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			margin: 80rpx auto 20rpx auto;
			font-size: 32rpx;
			width: 600rpx;
			height: 110rpx;
			border-radius: 50rpx;
			color: #ffffff;
			background-image: linear-gradient(to right, #7fb0ff, #78d5ff);
		}
	}
</style>
