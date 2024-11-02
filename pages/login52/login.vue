<template>
	<view class="content">
		<view class="logo-bg">
			<image mode="aspectFill"
				src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login52/login52_logo_bg.png">
			</image>
		</view>
		<view class="title">
			<text class="name">欢迎登录，</text>
			<text class="sub-name">内部系统</text>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image mode="aspectFill"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login52/login52_account.png">
				</image>
				<input placeholder="请输入账号" maxlength="12" />
			</view>
			<view class="row-input">
				<image mode="aspectFill"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login52/login52_password.png">
				</image>
				<input placeholder="输入密码" password maxlength="18" />
			</view>
			<view class="login-btn">登录</view>
			<view class="tip-link">
				查看<text>《演示效果》</text>和<text>《武林秘籍》</text>
			</view>
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
		background-color: #ebeffa;
	}

	.logo-bg {
		position: fixed;
		top: 10rpx;
		right: -10rpx;
		z-index: 99;

		image {
			width: 400rpx;
			height: 550rpx;
		}
	}

	.title {
		padding-left: 80rpx;
		padding-top: 100rpx;
		display: flex;
		justify-content: center;
		align-items: flex-start;
		flex-direction: column;
		height: 22vh;
		color: #444bbc;

		.name {
			font-size: 70rpx;
			line-height: 100rpx;
			font-weight: bold;
		}

		.sub-name {
			line-height: 80rpx;
			font-size: 45rpx;
			letter-spacing: 8rpx;
			font-weight: bold;
		}
	}

	.form-box {
		padding: 0rpx 80rpx;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 100rpx;
			background-color: #FFFFFF;
			border-radius: 12rpx;
			margin: 50rpx 0rpx;

			image {
				margin: 0 25rpx;
				flex-shrink: 0;
				width: 35rpx;
				height: 40rpx;
			}

			input {
				font-size: 30rpx;
				flex: 1;
			}
		}

		.login-btn {
			margin-top: 50rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			height: 90rpx;
			border-radius: 12rpx;
			background-color: #444bbc;
			color: #f4f4f4;
			font-size: 30rpx;
			letter-spacing: 10rpx;
			box-shadow: 0rpx 5rpx 20rpx #444bbc;
		}

		.tip-link {
			display: flex;
			justify-content: center;
			letter-spacing: 3rpx;
			line-height: 150rpx;
			font-size: 26rpx;

			text {
				color: #ff0000;
			}
		}

	}
</style>