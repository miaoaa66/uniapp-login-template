<template>
	<view class="content">
		<view class="login-form">
			<view class="book-clip-box" style="left: 140rpx;">
				<view class="line"></view>
				<image src="./static/login76_book_clip.png"></image>
			</view>
			<view class="book-clip-box" style="right: 140rpx;">
				<view class="line"></view>
				<image src="./static/login76_book_clip.png"></image>
			</view>
			<image class="logo" src="./static/login76_logo.png"></image>
			<view class="input-box">
				<image src="./static/login76_account.png"></image>
				<input placeholder="用户名" maxlength="16" />
			</view>
			<view class="input-box">
				<image src="./static/login76_password.png"></image>
				<input placeholder="密码" maxlength="16" />
			</view>
			<view class="login-btn">登录</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			};
		},
		onLoad() {
		},
		methods: {}
	};
</script>

<style lang="scss" scoped>
	page {
		background-color: #57cc71;
	}

	.login-form {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin: 200rpx auto;
		position: relative;
		width: 650rpx;
		padding: 100rpx 0;
		background-color: #f6f6f6;
		border: 20rpx solid #fff;

		.logo {
			width: 240rpx;
			height: 240rpx;
		}

		.book-clip-box {
			position: absolute;
			top: -65rpx;

			image {
				width: 60rpx;
				height: 60rpx;
			}

			.line {
				position: absolute;
				bottom: 60rpx;
				left: 28rpx;
				width: 4rpx;
				height: 300rpx;
				background-color: #ececec;

			}
		}

		.input-box {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			width: 520rpx;
			padding-top: 40rpx;
			height: 100rpx;
			border-bottom: 2rpx solid #aaaaaa;

			image {
				width: 40rpx;
				height: 40rpx;
				flex-shrink: 0;
			}

			input {
				padding-left: 20rpx;
				font-size: 30rpx;
			}
		}

		.login-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			margin-top: 80rpx;
			width: 520rpx;
			height: 90rpx;
			font-size: 36rpx;
			letter-spacing: 4rpx;
			border-radius: 10rpx;
			background: linear-gradient(0deg, #2fb465 0%, #4fc76e 100%) 50% 50%;
			color: #fff;
		}
	}
</style>