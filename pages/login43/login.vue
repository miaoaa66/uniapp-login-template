<template>
	<view class="content">
		<view class="form-box">
			<view class="title">CUSTOMER LOGIN</view>
			<view class="row-input">
				<image
					src="./static/login43_account.png">
				</image>
				<input placeholder="Username" maxlength="12" />
			</view>
			<view class="row-input">
				<image
					src="./static/login43_password.png">
				</image>
				<input placeholder="Password" password maxlength="18" />
			</view>
			<view class="login-btn">LOGIN</view>
			<view class="menu-link">
				<text>forgot password</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #689eba;
	}

	.form-box {
		margin: 300rpx 80rpx 0rpx 80rpx;
		background-color: #FFFFFF;

		.title {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 140rpx;
			font-size: 30rpx;
			letter-spacing: 4rpx;
			color: #FFFFFF;
			background-color: #25577c;
		}

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			margin: 0 40rpx;
			height: 140rpx;
			border-bottom: 2rpx solid #c6c6c6;

			image {
				margin-right: 40rpx;
				width: 30rpx;
				height: 30rpx;
				flex-shrink: 0;
			}

			input {
				flex: 1;

			}
		}

		.login-btn {
			position: relative;
			display: inline-block;
			width: calc(100% + 60rpx);
			height: 100rpx;
			line-height: 50px;
			text-align: center;
			margin-left: -30rpx;
			margin-right: -30rpx;
			letter-spacing: 4rpx;
			color: #FFFFFF;
			background-color: #25577c;
			top: 20rpx;
		}

		.login-btn:before {
			content: "";
			position: absolute;
			height: 0;
			width: 0;
			border-top: 30rpx solid #143151;
			border-left: 30rpx solid transparent;
			bottom: -30rpx;
			left: 0;
		}

		.login-btn:after {
			content: "";
			position: absolute;
			height: 0;
			width: 0;
			border-top: 30rpx solid #143151;
			border-right: 30rpx solid transparent;
			right: 0;
			bottom: -30rpx;
		}

		.menu-link {
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 26rpx;
			color: #cccccc;
			height: 80rpx;
		}
	}
</style>