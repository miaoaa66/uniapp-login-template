<template>
	<view class="content">
		<view class="head">
			<image class="logo" mode="aspectFit" src="./static/login66_logo.png">
			</image>
			<text class="name">www.xxxx.com</text>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image src="./static/login66_account.png">
				</image>
				<input placeholder="Email address" maxlength="16" />
			</view>
			<view class="row-input">
				<image src="./static/login66_password.png">
				</image>
				<input placeholder="Password" password maxlength="18" />
			</view>
			<view class="login-btn">Login</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				logo: "",
				email: "",
				password: "",
			}
		},
		methods: {
			// 去注册
			goToRegister() {
				uni.navigateTo({
					url: '/pages/login6/register'
				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-image: linear-gradient(#2c221e, #283037);
	}

	.head {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 20vh;

		.logo {
			width: 100rpx;
			height: 100rpx;
		}

		.name {
			margin-left: 30rpx;
			font-size: 40rpx;
			color: #f1f1f3;
			font-weight: bold;
		}
	}

	.form-box {
		padding: 0rpx 80rpx;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			margin: 6rpx 0;
			height: 120rpx;
			border: 1rpx solid #29231e;
			background-color: #303131;
			border-radius: 10rpx;

			image {
				width: 40rpx;
				height: 40rpx;
				padding: 0 20rpx;
			}

			input {
				flex: 1;
			}
		}

		.login-btn {
			margin-top: 30rpx;
			width: 100%;
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 32rpx;
			height: 120rpx;
			background-color: #f27d1f;
			letter-spacing: 10rpx;
			border-radius: 10rpx;
			color: #f1f1f3;
		}
	}
</style>