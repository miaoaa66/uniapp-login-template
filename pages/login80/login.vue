<template>
	<view class="content">
		<view class="title">
			<image class="img" src="./static/login80_bg.png"></image>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image class="icon" src="./static/login80_account.png"></image>
				<input placeholder="请输入登录账号" maxlength="12" />
			</view>
			<view class="row-input">
				<image class="icon" src="./static/login80_password.png"></image>
				<input placeholder="请输入账号密码" maxlength="18" />
			</view>
			<view class="menu-link">
				<text>忘记密码</text>
			</view>
			<view class="login-btn">登录</view>
		</view>
		<view class="shortcut">
			<view class="tips">
				<text class="line"></text>
				快捷登录
				<text class="line"></text>
			</view>
			<view class="icon-box">
				<image src="./static/login80_vx.png"></image>
				<image src="./static/login80_zfb.png"></image>
				<image src="./static/login80_wb.png"></image>
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

			// 密码图标切换
			passwordChange() {
				this.showPassword = !this.showPassword
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #fdebd7;
	}

	.title {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		height: 30vh;

		.img {
			width: 85%;
			height: 85%;
		}
	}

	.form-box {
		padding: 0rpx 60rpx;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 100rpx;
			margin: 50rpx 0rpx;
			border-bottom: 2rpx solid #bfbfbf;

			.icon {
				margin-right: 20rpx;
				flex-shrink: 0;
				width: 35rpx;
				height: 35rpx;
			}

			input {
				font-size: 30rpx;
				flex: 1;
			}
		}

		.menu-link {
			display: flex;
			justify-content: flex-end;
			align-items: center;
			color: #868480;
			font-size: 26rpx;
		}

		.login-btn {
			margin-top: 50rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			height: 90rpx;
			border-radius: 50rpx;
			background-color: #1b1b1b;
			color: #ffffff;
			font-size: 30rpx;
			letter-spacing: 5rpx;
		}

	}

	.shortcut {
		display: flex;
		flex-direction: column;

		.tips {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 100rpx;
			font-size: 26rpx;

			.line {
				width: 60rpx;
				height: 2rpx;
				margin: 0 40rpx;
				background-color: #dad9d9;
			}
		}

		.icon-box {
			height: 140rpx;
			display: flex;
			justify-content: center;
			align-items: center;

			image {
				background-color: #ffffff;
				border-radius: 100%;
				margin: 0 40rpx;
				width: 80rpx;
				height: 80rpx;
			}
		}
	}
</style>