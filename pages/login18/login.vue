<template>
	<view class="content">
		<view class="logo">
			<text class="txt">LOGO</text>
			<text class="tip">这里是一句话</text>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image
					src="./static/login18_phone.png">
				</image>
				<input placeholder="请输入手机号" maxlength="11" placeholder-class="placeholder" />
			</view>
			<view class="row-input">
				<image
					src="./static/login18_password.png">
				</image>
				<input placeholder="请输入密码" maxlength="18" password placeholder-class="placeholder" />
			</view>
			<view class="login-btn">立即登录</view>
			<view class="menu-link">
				<text>注册账号</text>
				<text>忘记密码</text>
			</view>
		</view>

		<view class="bottom-box">
			<image
				src="./static/login18_bg.png">
			</image>
		</view>
	</view>
</template>

<script>
</script>

<style lang="scss">
	page {
		background-color: #ececee;
	}

	.logo {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 25vh;
		font-size: 60rpx;

		.txt {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 160rpx;
			height: 160rpx;
			color: #FFFFFF;
			font-size: 40rpx;
			font-weight: bold;
			letter-spacing: 5rpx;
			border-radius: 20rpx;
			background-color: #73a858;
		}

		.tip {
			letter-spacing: 10rpx;
			font-size: 35rpx;
			line-height: 100rpx;
			color: #73a858;
		}
	}

	.form-box {
		padding: 20rpx 60rpx;

		.row-input {
			margin: 30rpx 0rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;
			padding: 0 20rpx;
			height: 100rpx;
			border: 1rpx solid #bbd1b2;
			border-radius: 20rpx;

			image {
				padding: 0rpx 30rpx 0rpx 10rpx;
				width: 25rpx;
				height: 29rpx;
				flex-shrink: 0;
			}

			.placeholder {
				font-size: 30rpx;
				color: #73a858;
			}

			input {
				font-size: 30rpx;
				padding: 0rpx 40rpx;
				color: #73a858;
				border-left: 3rpx solid #73a858;
			}
		}

		.login-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 35rpx;
			height: 100rpx;
			border-radius: 20rpx;
			color: #FFFFFF;
			background-color: #73a858;
		}

		.menu-link {
			display: flex;
			justify-content: space-between;
			font-size: 28rpx;
			color: #73a858;
			letter-spacing: 3rpx;
			line-height: 80rpx;
		}

	}


	.bottom-box {
		position: fixed;
		bottom: 0;
		width: 100%;
		height: 100%;
		height: 35vh;

		image {
			width: 100%;
			height: 100%;
		}
	}
</style>
