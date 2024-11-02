<template>
	<view class="form-box">
		<view class="title">MEMBER LOGIN</view>
		<view class="row-input">
			<view class="image-box">
				<image mode="aspectFill"
					src="./static/login45_account.png">
				</image>
			</view>
			<view class="input-box">
				<input type="text" placeholder="Username" maxlength="18" />
			</view>
		</view>
		<view class="row-input">
			<view class="image-box">
				<image mode="aspectFit"
					src="./static/login45_password.png">
				</image>
			</view>
			<view class="input-box">
				<input type="text" placeholder="Password" password="true" maxlength="16" />
			</view>
		</view>
		<view class="btn-box">
			<view class="btn login">Login</view>
		</view>
		<view class="menu-link">
			<text>register</text>
			<text>Forgot Password</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
	}
</script>

<style lang="scss">
	page {
		background-color: #8a3516;
	}

	.form-box {
		position: fixed;
		background: inherit;
		top: 20vh;
		left: 50rpx;
		right: 50rpx;
		padding: 60rpx;
		border-radius: 20rpx;
		border: 4rpx solid #d1b0af;

		.title {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 100rpx;
			letter-spacing: 6rpx;
			font-size: 34rpx;
			color: #FFFFFF;
			font-weight: bold;
		}


		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			margin: 30rpx 0rpx;
			height: 100rpx;
			font-size: 28rpx;
			border-radius: 10rpx;

			.image-box {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 20%;
				height: 100%;
				border-top-left-radius: 16rpx;
				border-bottom-left-radius: 16rpx;
				background-color: #cf4930;
			}

			image {
				width: 60rpx;
				height: 60rpx;
			}

			.input-box {
				flex: 1;
				height: 100%;
				display: flex;
				justify-content: center;
				align-items: center;
				border-top-right-radius: 16rpx;
				border-bottom-right-radius: 16rpx;
				background-color: #f6eeee;
			}
		}

		.btn-box {
			padding: 20rpx 0rpx 20rpx 0rpx;

			.btn {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 100%;
				height: 100rpx;
				border-radius: 16rpx;
				font-size: 35rpx;
				letter-spacing: 5rpx;
			}

			.login {
				color: #FFFFFF;
				background-color: #ee7061;
			}
		}

		.menu-link {
			display: flex;
			justify-content: space-between;
			align-items: center;
			font-size: 26rpx;
			color: #d1b0af;
		}
	}
</style>