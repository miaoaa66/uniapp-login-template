<template>
	<view class="content">
		<view class="form-box">
			<view class="title-box">
				<text class="title">Login Form</text>
				<text class="title-sub">Fill out the form below to login</text>
			</view>
			<view class="input-box">
				<view class="row-input border-bottom">
					<input placeholder="Email" maxlength="16" />
				</view>
				<view class="row-input">
					<input placeholder="Password" password maxlength="18" />
					<image mode="aspectFill"
						src="./static/login55_password.png">
					</image>
				</view>
			</view>
			<view class="login-btn">LOGIN</view>
			<view class="menu-link">
				<view class="row">
					<checkbox class="remember" value="" color="#ff8b00" style="transform:scale(0.7)" />
					<text>Remember me</text>
				</view>
				<view class="row">forgot password</view>
			</view>
			<view class="form-bottom"></view>
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
		background-color: #4a87b3;
	}

	.form-box {
		position: relative;
		margin: 300rpx 80rpx 0rpx 80rpx;
		border-top-left-radius: 20rpx;
		border-top-right-radius: 20rpx;
		background-color: #eaefe9;

		.title-box {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			.title {
				display: flex;
				justify-content: center;
				align-items: flex-end;
				width: 100%;
				height: 120rpx;
				font-weight: bold;
				font-size: 40rpx;
				letter-spacing: 4rpx;
				color: #656564;
			}

			.title-sub {
				padding-top: 20rpx;
				text-align: center;
				width: 100%;
				height: 40rpx;
				font-size: 22rpx;
				color: #b9bdbf;
			}
		}

		.input-box {
			margin: 20rpx 40rpx;
			border-radius: 10rpx;
			background-color: #e6e7eb;

			.row-input {
				display: flex;
				justify-content: flex-start;
				align-items: center;
				padding: 0 20rpx;
				height: 100rpx;
				border-radius: 10rpx;
				border: 1rpx solid #dddfe2;

				image {
					width: 30rpx;
					height: 30rpx;
					flex-shrink: 0;
				}

				input {
					flex: 1;
				}
			}

			.border-bottom {
				border-bottom: 1rpx solid #dddfe2;
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
			background-color: #ff8b00;
			border-radius: 10rpx;
			top: 20rpx;
		}

		.menu-link {
			display: flex;
			justify-content: space-around;
			align-items: center;
			font-size: 26rpx;
			color: #707370;
			line-height: 120rpx;
		}

		.form-bottom {
			position: absolute;
			width: 100%;
			height: 30rpx;
			border-bottom-left-radius: 20rpx;
			border-bottom-right-radius: 20rpx;
			background-color: #ed7900;
		}
	}
</style>