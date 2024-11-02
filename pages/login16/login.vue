<template>
	<view class="content">
		<view class="upper">
			<image mode="widthFix" src="./static/login16_bg.png"></image>
		</view>
		<view class="logo">LOGO</view>
		<view class="form-box">
			<view class="row-input">
				<input type="text" placeholder="请输入手机号/邮箱" maxlength="18" />
			</view>
			<view class="row-input">
				<input type="text" placeholder="验证码" maxlength="6" />
				<view class="code">
					<text>获取验证码</text>
				</view>
			</view>
			<view class="btn-box">
				<view class="btn login">登录</view>
				<view class="btn close">取消</view>
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
	.upper {
		width: 100%;
		height: 40vh;

		image {
			width: 100%;
			height: 100%;
		}
	}

	.logo {
		position: fixed;
		display: flex;
		justify-content: center;
		align-items: center;
		top: 10vh;
		left: 300rpx;
		width: 150rpx;
		height: 150rpx;
		border-radius: 100%;
		font-weight: bold;
		font-size: 40rpx;
		background-color: #ffffff;
		color: #ff6c62;
		border: 2rpx solid #ff0000;
		box-sizing: border-box;
	}

	.form-box {
		position: fixed;
		top: 27vh;
		left: 50rpx;
		right: 50rpx;
		padding: 60rpx 60rpx 0rpx 60rpx;
		border-radius: 30rpx;
		background-color: #FFFFFF;
		box-shadow: 0rpx 10rpx 15rpx #ebd7da;

		.row-input {
			display: flex;
			justify-content: space-between;
			align-items: center;
			height: 140rpx;
			border-bottom: 1rpx solid #f4f4f4;


			.code {
				display: flex;
				justify-content: center;
				width: 180rpx;
				font-size: 28rpx;
				color: #fe9a85;
			}
		}

		.btn-box {
			padding: 60rpx 0rpx;

			.btn {
				margin: 40rpx 0rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				width: 100%;
				height: 100rpx;
				border-radius: 30rpx;
				font-size: 35rpx;
				letter-spacing: 5rpx;
			}

			.login {
				color: #FFFFFF;
				background: linear-gradient(to right, #fe9378, #fc675d)
			}

			.close {
				border: 1rpx solid #f0f0f0;
				box-sizing: border-box;
			}
		}

	}
</style>
