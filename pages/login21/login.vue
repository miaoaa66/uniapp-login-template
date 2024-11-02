<template>
	<view class="content">
		<view class="title">登录</view>
		<view class="head">
			<image src="./static/login21_bg.png"></image>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image src="./static/login21_phone.png"></image>
				<input maxlength="11" placeholder="手机号" />
			</view>
			<view class="row-input">
				<image src="./static/login21_password.png"></image>
				<input maxlength="11" placeholder="密码" />
			</view>
			<view class="login-btn">登录</view>
			<view class="menu-link">
				<text>账号密码登录</text>
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
		background-color: #d9e4e8;
	}

	.title {
		line-height: 100rpx;
		font-size: 50rpx;
		font-weight: bold;
		padding-left: 50rpx;
	}

	.head {
		display: flex;
		justify-content: center;
		align-items: flex-end;
		width: 100%;
		height: 35vh;

		image {
			width: 540rpx;
			height: 400rpx;
		}
	}

	.form-box {
		padding: 40rpx 120rpx 0rpx 120rpx;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 100rpx;
			margin: 40rpx 0rpx;
			border-radius: 15rpx;
			background-color: #FFFFFF;

			image {
				margin: 0rpx 40rpx;
				flex-shrink: 0;
				width: 25rpx;
				height: 30rpx;
			}
		}

		.login-btn {
			height: 100rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			border-radius: 15rpx;
			font-size: 32rpx;
			letter-spacing: 5rpx;
			color: #e0e7eb;
			background-color: #013c5c;
		}
		
		.menu-link{
			display: flex;
			justify-content: flex-start;
			height: 100rpx;
			align-items: center;
			font-size: 26rpx;
			color: #4c778c;
		}
	}
</style>
