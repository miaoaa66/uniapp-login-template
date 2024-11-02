<template>
	<view>
		<image class="bg"  mode="heightFix" src="./static/login11_bg.png"></image>
		<view class="logo-box">
			<view class="logo">LOGO</view>
		</view>
		<view class="row-input">
			<image src="./static/login11_account.png"></image>
			<input placeholder="Username" maxlength="12" placeholder-style="color: #FFFFFF;font-size:30rpx" />
		</view>
		<view class="row-input">
			<image src="./static/login11_password.png"></image>
			<input placeholder="Password" maxlength="18" password placeholder-style="color: #FFFFFF;font-size:30rpx;" />
		</view>
		<text class="forget-link">Forget Password</text>
		<view class="bottom-box">
			<view class="login-btn">Sign In</view>
			<view class="link-tip">
				Don`t have an account?<text @click="gotoRegister">Sign Up</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		methods: {
			// 去注册
			gotoRegister() {
				uni.navigateTo({
					url: '/pages/login11/register'
				})
			}
		}
	}
</script>

<style lang="scss">
	.bg {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: -1;
		width: 100%;
		height: 100%;
	}

	.logo-box {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 45vh;

		.logo {
			display: flex;
			justify-content: center;
			align-items: center;
			border-radius: 100%;
			width: 280rpx;
			height: 280rpx;
			font-size: 60rpx;
			font-weight: bold;
			color: #fe3366;
			background-color: #FFFFFF;
		}
	}

	.row-input {
		padding: 50rpx 60rpx 0 60rpx;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		height: 100rpx;
		border-bottom: 1rpx solid #818181;

		image {
			width: 40rpx;
			height: 40rpx;
		}

		input {
			padding: 0 40rpx;
			color: #FFFFFF;
		}
	}

	.forget-link {
		display: flex;
		justify-content: flex-end;
		padding-right: 40rpx;
		line-height: 80rpx;
		color: #c0bfbd;
		font-size: 30rpx;
	}

	.bottom-box {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;

		.login-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 150rpx;
			font-size: 38rpx;
			letter-spacing: 5rpx;
			color: #FFFFFF;
			background-color: #fc3166;
		}

		.link-tip {
			display: flex;
			justify-content: center;
			align-items: center;
			line-height: 120rpx;
			font-size: 30rpx;
			color: #c0bfbd;

			text {
				padding-left: 20rpx;
				color: #FFFFFF;
			}
		}
	}
</style>
