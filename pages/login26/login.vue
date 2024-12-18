<template>
	<view class="content">
		<view class="head">
			<image src="./static/login26_bg.png"></image>
		</view>
		<view class="title">
			<text class="name">Hello!</text>
			<text class="sub-name">欢迎登录XX系统</text>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image src="./static/login26_account.png"></image>
				<input placeholder="请输入账号" maxlength="12" />
			</view>
			<view class="row-input">
				<image src="./static/login26_password.png"></image>
				<input placeholder="输入密码" password maxlength="18" />
			</view>
			<view class="menu-link">
				<text>注册</text>
				<text>忘记密码?</text>
			</view>
			<view class="login-btn">登录</view>
			<view class="tip-link">
				登录即代表你已同意<text>用户协议</text>和<text>隐私政策</text>
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

<style lang="scss" scoped>
	page {
		background-color: #e9edf6;
	}

	.head {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		width: 100%;
		height: 30vh;
		z-index: -1;

		image {
			width: 100%;
			height: 100%;
		}
	}

	.title {
		padding-left: 80rpx;
		padding-top: 100rpx;
		display: flex;
		justify-content: center;
		align-items: flex-start;
		flex-direction: column;
		height: 30vh;

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
				width: 31rpx;
				height: 35rpx;
			}

			input {
				font-size: 30rpx;
				flex: 1;
			}
		}

		.menu-link {
			display: flex;
			justify-content: space-between;
			align-items: center;
			font-size: 26rpx;
		}

		.login-btn {
			margin-top: 50rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			height: 90rpx;
			border-radius: 12rpx;
			background-color: #819682;
			color: #f4f4f4;
			font-size: 30rpx;
			letter-spacing: 5rpx;
			box-shadow: 0rpx 5rpx 20rpx #94ac95;
		}

		.tip-link {
			display: flex;
			justify-content: center;
			letter-spacing: 3rpx;
			line-height: 150rpx;
			font-size: 26rpx;

			text {
				color: #57a2ee;
			}
		}

	}
</style>
