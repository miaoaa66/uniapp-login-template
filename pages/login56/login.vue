<template>
	<view class="content">
		<view class="form-box">
			<view class="tip">Login Form</view>
			<view class="input-box input-margin">
				<input placeholder="Username" maxlength="16" />
				<image mode="aspectFill"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login56/login56_account.png">
				</image>
			</view>
			<view class="input-box">
				<input placeholder="Password" maxlength="18" password="true" />
				<image mode="aspectFill"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login56/login56_password.png">
				</image>
			</view>
			<view class="btn-box">
				<view class="btn">Sign In</view>
				<text class="btn-tip">Lost Your Password?</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		methods: {}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #502e53;
	}

	.content {
		display: flex;
		flex-direction: column;
	}

	.form-box {
		position: relative;
		display: flex;
		flex-direction: column;
		margin: 50% auto;
		border-radius: 10rpx;
		width: 650rpx;
		background-color: #f8fafb;

		.tip {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			padding-left: 40rpx;
			height: 120rpx;
			font-size: 30rpx;
			color: #f8fafb;
			background-color: #6c4a6f;
		}

		.input-box {
			display: flex;
			justify-content: space-between;
			align-items: center;
			margin: 20rpx 40rpx;
			border-radius: 10rpx;
			background-color: #efedf0;
			border: 1rpx solid #e2e4e5;
			padding: 0 20rpx;

			input {
				padding: 0 20rpx;
				flex: 1;
				height: 100rpx;
				font-size: 30rpx;
			}

			image {
				width: 30rpx;
				height: 30rpx;
				flex-shrink: 0;
			}
		}

		.input-margin {
			margin-top: 60rpx;
		}

		.btn-box {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			margin: 50rpx;
			height: 80rpx;

			.btn {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 200rpx;
				height: 80rpx;
				font-size: 28rpx;
				color: #f1f2f3;
				background-color: #6e4a70;
				border-radius: 6rpx;
			}

			.btn-tip {
				margin-left: 40rpx;
				font-size: 24rpx;
				color: #a3a3a3;
			}
		}

	}
</style>