<template>
	<view class="form-box">
		<view class="input-box">
			<view class="img">
				<image mode="aspectFill"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login48/login48_account.png">
				</image>
			</view>
			<input placeholder="Email ID" maxlength="11" />
		</view>
		<view class="input-box">
			<view class="img">
				<image mode="aspectFill"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login48/login48_password.png">
				</image>
			</view>
			<input placeholder="Password" maxlength="18" password="true" />
		</view>
		<view class="link-box">
			<text>Forgot Password?</text>
		</view>
		<view class="title btn">
			USER LOGIN
		</view>
		<view class="login btn">
			LOGIN
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

<style lang="scss">
	page {
		background-color: #5877ba;
	}

	.content {
		display: flex;
		flex-direction: column;
	}

	.form-box {
		position: relative;
		display: flex;
		flex-direction: column;
		margin: 30vh auto;
		padding: 100rpx 0;
		border-radius: 20rpx;
		width: 650rpx;
		background-color: #e7e8ea;
		border: 2rpx solid #ccced0;

		.input-box {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			margin: 20rpx 50rpx;
			border-radius: 10rpx;
			background-color: #bed1df;
			border: 6rpx solid #5877ba;
			border-radius: 50rpx;

			.img {
				display: flex;
				justify-content: center;
				align-items: center;
				margin: 0 20rpx;
				width: 60rpx;
				height: 60rpx;
				border-radius: 100%;
				background-color: #d7e5ee;

				image {
					width: 30rpx;
					height: 30rpx;
				}
			}



			input {
				padding: 0 20rpx;
				height: 80rpx;
			}
		}

		.title {
			top: -60rpx;
			left: 20%;
			border: 10rpx solid #5877ba;
			background-color: #45c6e5;
		}


		.login {
			bottom: -60rpx;
			left: 20%;
			border: 10rpx solid #5877ba;
			background-color: #f281ac;
		}

		.btn {
			position: absolute;
			display: flex;
			justify-content: center;
			align-items: center;
			width: 60%;
			height: 80rpx;
			font-size: 40rpx;
			font-weight: bold;
			border-radius: 50rpx;
			color: #fbfbfb;

			&::before {
				content: "";
				position: absolute;
				top: 0;
				left: 0;
				right: 0;
				bottom: 0;
				border: 2rpx solid #f8fafb;
				border-radius: 50rpx;
			}
		}

		.btn-box {
			position: absolute;
			display: flex;
			justify-content: space-between;
			align-items: center;
			margin: 50rpx;
			height: 80rpx;
		}

		.link-box {
			display: flex;
			justify-content: flex-end;
			align-items: center;
			width: 100%;
			height: 80rpx;
			font-size: 28rpx;
			color: #b4b6b7;

			text {
				margin-right: 60rpx;
			}
		}
	}
</style>