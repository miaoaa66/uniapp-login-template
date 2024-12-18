<template>
	<view class="content">
		<view class="head">
			<view class="logo-box">
				<image class="logo" mode="aspectFill" src="@/static/logo.png">
				</image>
				<text class="tip">COMET</text>
			</view>
			<text class="name">Welcome!</text>
		</view>
		<view class="form-box">
			<view class="row-input">
				<input placeholder="Email Address" placeholder-class="input-placeholder" maxlength="16" />
			</view>
			<view class="row-input">
				<input placeholder="Password" placeholder-class="input-placeholder" maxlength="18" password />
			</view>
			<view class="row-input">
				<input placeholder="Confirm Password" placeholder-class="input-placeholder" maxlength="18" password />
			</view>
			<view class="login-btn">
				Continue
			</view>
			<view class="menu-link" @click="gotoRegister">
				You don't have an account?
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				logo: "",
			}
		},
		methods: {
			gotoRegister() {
				uni.redirectTo({
					url: "/pages/login59/register"
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.head {
		height: 25vh;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;

		.logo-box {
			display: flex;
			justify-content: center;
			align-items: center;

			.logo {
				width: 80rpx;
				height: 80rpx;
			}

			.tip {
				padding-left: 20rpx;
				font-size: 60rpx;
				font-weight: bold;
				color: #403b51;
				letter-spacing: 4rpx;
			}
		}

		.name {
			padding-top: 80rpx;
			font-size: 40rpx;
			letter-spacing: 2rpx;
			color: #a8a8a8;
			font-weight: bold;
		}
	}

	.form-box {
		padding: 0 80rpx;
		height: 100%;

		.row-input {
			margin-top: 50rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;
			color: #d1ccc3;
			height: 100rpx;
			font-size: 30rpx;
			border-bottom: 4rpx solid #c1c1c1;

			image {
				padding: 0rpx 40rpx;
				width: 40rpx;
				height: 40rpx;
			}

			.input-placeholder {
				font-size: 28rpx;
				color: #adadad;
			}
		}

		.login-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			color: #fff;
			margin: 80rpx auto;
			width: 400rpx;
			height: 100rpx;
			font-size: 32rpx;
			border-radius: 50rpx;
			background-color: #0f1026;
		}

		.menu-link {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 40rpx;
			color: #adadad;
		}
	}
</style>