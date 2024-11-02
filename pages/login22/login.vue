<template>
	<view class="content">
		<view class="title">欢迎登录XX系统</view>
		<view class="form-box">
			<image class="bg" src="./static/login22_bg.png"></image>
			<view class="form">
				<view class="row-input">
					<image src="./static/login22_phone.png"></image>
					<input placeholder="请输入手机号" placeholder-class="placeholder"  maxlength="11" />
				</view>
				<view class="row-input">
					<image src="./static/login22_password.png"></image>
					<input placeholder="请输入密码" placeholder-class="placeholder" maxlength="18" password  />
				</view>
				<view class="login-btn">登录</view>
			</view>
		</view>
		<view class="menu-link">
			<text style="border-right: 1rpx solid #9fa9c7;">忘记密码</text>
			<text style="border-left: 1rpx solid #9fa9c7;">用户注册</text>
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
		background-color: #1f265a;
	}

	.form-box {
		position: relative;

		.bg {
			position: absolute;
			padding-left: 50rpx;
			display: flex;
			justify-content: center;
			position: relative;
			width: 650rpx;
			height: 600rpx;
		}

		.form {
			position: absolute;
			top: 100rpx;
			left: 120rpx;
			right: 120rpx;

			.row-input {
				padding-top: 40rpx;
				width: 100%;
				display: flex;
				justify-content: flex-start;
				align-items: center;
				height: 80rpx;
				margin-bottom: 40rpx;
				border-bottom: 1rpx solid #576294;

				image {
					margin: 0rpx 20rpx 0rpx 10rpx;
					flex-shrink: 0;
					width: 27rpx;
					height: 32rpx;
				}
				
				.placeholder{
					font-size: 28rpx;
					color: #6572ac;
				}
				
			}

			.login-btn {
				display: flex;
				justify-content: center;
				align-items: center;
				height: 100rpx;
				color: #c2cbda;
				font-size: 35rpx;
				letter-spacing: 5rpx;
				border-radius: 20rpx;
				background-color: #2a3e85;
				border: 1rpx solid #467ee4;
				box-sizing: border-box;
			}
		}
	}

	.title {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		padding-left: 80rpx;
		height: 200rpx;
		font-size: 45rpx;
		letter-spacing: 5rpx;
		color: #eef8fd;
	}

	.menu-link {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 120rpx;
		font-size: 28rpx;
		color: #9fa9c7;

		text {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 25rpx;
			width: 180rpx;
		}
	}
</style>
