<template>
	<view class="content">
		<view class="logo-box">
			<view class="logo"></view>
			<view class="name">LOGO</view>
		</view>
		<view class="info-box">
			<view class="item">
				<image src="./static/login78_account.png"></image>
				<input placeholder="请输入您的手机号" maxlength="11" placeholder-class="placeholder-input" />
			</view>
			<view class="item">
				<image src="./static/login78_password.png"></image>
				<input :password="true" maxlength="20" placeholder="请输入密码" placeholder-class="placeholder-input" />
			</view>
			<view class="forget-pass">忘记密码?</view>
			<view class="btn-box">
				<view class="login-btn">登 录</view>
				<view class="register-btn">注册</view>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
			}
		},
		methods: {

		}
	}
</script>
<style lang="scss" scoped>
	page {
		background-color: #faca10;
	}

	.logo-box {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		width: 100%;
		margin: 100rpx 0;
	}

	.logo {
		width: 200rpx;
		height: 200rpx;
		border-radius: 100%;
		background-color: #fff795;
		box-shadow: 0 0 0 1rpx #fff795, 0 0 0 5rpx #ffd61b inset;
	}

	.name {
		padding-top: 30rpx;
		font-weight: bold;
		font-size: 100rpx;
		color: #fff795;
	}

	.info-box {
		margin: 100rpx;

		.item {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			margin: 30rpx 0;
			width: 100%;
			border-radius: 50rpx;
			background-color: #ffdf2a;
			height: 90rpx;

			.placeholder-input {
				font-size: 28rpx;
				color: #dab20e;
			}

			image {
				margin: 0 30rpx;
				width: 30rpx;
				height: 30rpx;
			}

			input {
				flex: 1;
				font-size: 28rpx;
				color: #dab20e;
			}
		}

		.forget-pass {
			padding-right: 30rpx;
			display: flex;
			justify-content: flex-end;
			font-size: 26rpx;
			letter-spacing: 5rpx;
		}

		.btn-box {
			display: flex;
			justify-content: center;
			align-items: center;
			flex-direction: column;

			.login-btn {
				width: 350rpx;
				height: 80rpx;
				background-color: #fcda55;
				display: flex;
				justify-content: center;
				align-items: center;
				border-radius: 50rpx;
				font-size: 40rpx;
				margin: 50rpx 0;
				box-shadow: 0rpx 10rpx 20rpx #cf9305;
			}

			.register-btn {
				height: 100rpx;
				width: 100%;
				display: flex;
				justify-content: center;
				align-items: center;
			}
		}
	}
</style>