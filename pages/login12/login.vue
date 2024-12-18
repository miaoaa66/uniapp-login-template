<template>
	<view class="content">
		<view class="head">
			<image src="./static/login12_bg .png"></image>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image src="./static/login12_account.png"></image>
				<input placeholder="请输入用户名" maxlength="12" placeholder-class="placeholder" />
			</view>
			<view class="row-input">
				<image src="./static/login12_password.png"></image>
				<input placeholder="请输入密码" password maxlength="18" placeholder-class="placeholder" />
			</view>
			<view class="login-btn">
				登录
			</view>
			<view class="menu-link">
				<text>立即注册</text>
				<text>忘记密码</text>
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
		background-color: #2f3640;
	}

	.head {
		width: 100%;
		height: 50vh;

		image {
			width: 100%;
			height: 100%;
		}
	}

	.form-box {
		padding: 0 80rpx;

		.row-input {
			margin: 40rpx 0rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 100rpx;
			background-color: #d39e6e;

			image {
				padding: 0 40rpx;
				width: 35rpx;
				height: 35rpx;
			}

			input {
				color: #e5e5e5;
				font-size: 28rpx;
			}
			
			.placeholder{
				color: #e5e5e5;
				font-size: 28rpx;
			}
		}

		.login-btn {
			margin-top: 30rpx;
			height: 100rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			color: #474d55;
			background-color: #e5e5e5;
		}

		.menu-link {
			display: flex;
			justify-content: space-between;
			line-height: 80rpx;
			font-size: 28rpx;
			color: #e5e5e5;
		}
	}
</style>
