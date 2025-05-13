<template>
	<view class="content">
		<view class="head-box">
			<view class="title">后台登录系统</view>
			<image class="head" src="./static/login84_head.png"></image>
			<image class="logo" src="./static/login84_logo.png" mode="aspectFit"></image>
		</view>
		<view class="info-box">
			<view class="item">
				<image src="./static/login84_account.png"></image>
				<input placeholder="请输入您的手机号" maxlength="11" placeholder-class="placeholder-input" />
			</view>
			<view class="forget-pass">联系管理人员</view>
			<view class="btn-box">
				<view class="login-btn">登录</view>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {}
		},
		onLoad() {
		},
		methods: {

		}
	}
</script>
<style lang="scss" scoped>
	page {
		background-color: #3eebe9;
	}

	.head-box {
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
		align-items: center;
		width: 100%;
		height: 40vh;

		.title {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 20vh;
			font-size: 40rpx;
			letter-spacing: 4rpx;
			color: #fff;
			font-weight: bold;
			background-color: #f7747a;
		}

		.head {
			width: 100%;
			height: 20vh;
		}
		
		.logo {
			width: 300rpx;
			height: 300rpx;
		}
	}

	.info-box {
		margin: 75rpx;

		.item {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			margin: 30rpx 0;
			width: 100%;
			border-radius: 50rpx;
			background-color: #fff;
			height: 110rpx;

			.placeholder-input {
				font-size: 28rpx;
			}

			image {
				margin: 0 30rpx;
				width: 30rpx;
				height: 30rpx;
			}

			input {
				flex: 1;
				font-size: 28rpx;
			}
		}

		.forget-pass {
			display: flex;
			justify-content: flex-end;
			font-size: 26rpx;
		}

		.btn-box {
			display: flex;
			justify-content: center;
			align-items: center;
			flex-direction: column;

			.login-btn {
				width: 600rpx;
				height: 110rpx;
				background-color: #0b2e52;
				color: #fff;
				display: flex;
				justify-content: center;
				align-items: center;
				border-radius: 100rpx;
				font-size: 30rpx;
				margin: 50rpx 0;
			}
		}
	}
</style>