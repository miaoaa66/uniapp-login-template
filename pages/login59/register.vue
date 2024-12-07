<template>
	<view class="content">
		<view class="head">
			<view class="logo-box">
				<image class="logo" mode="aspectFill" src="@/static/logo.png">
				</image>
				<text class="tip">COMET</text>
			</view>
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
			<view class="row-input">
				<input placeholder="DOB" placeholder-class="input-placeholder" maxlength="18" password />
			</view>
			<view class="row-input">
				<input placeholder="Address" placeholder-class="input-placeholder" maxlength="18" password />
			</view>
			<view class="login-btn">
				<view class="login btn" @click="gotoLogin">Login</view>
				<view class="register btn">Submit</view>
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
			gotoLogin() {
				uni.redirectTo({
					url: "/pages/login59/login"
				})
			}
		}
	}
</script>

<style lang="scss">
	.head {
		height: 10vh;
		display: flex;
		justify-content: flex-end;
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

			.btn {
				display: flex;
				justify-content: center;
				align-items: center;
				margin: 80rpx auto;
				width: 240rpx;
				height: 100rpx;
				font-size: 32rpx;
				border-radius: 50rpx;
			}

			.login {
				box-sizing: border-box;
				color: #0f1026;
				border: 2rpx solid #0f1026;
			}

			.register {
				color: #c1c1c1;
				background-color: #0f1026;
			}
		}
	}
</style>