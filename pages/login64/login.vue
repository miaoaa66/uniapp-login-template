<template>
	<view class="content">
		<view class="head">
			<image class="logo" mode="aspectFill" src="@/static/logo.png"></image>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image mode="aspectFit" src="@/static/logo.png">
				</image>
				<input placeholder="请输入手机号" placeholder-class="input-placeholder" maxlength="18" />
			</view>
			<view class="row-input">
				<image mode="aspectFit" src="@/static/logo.png">
				</image>
				<input placeholder="请输入密码" placeholder-class="input-placeholder" maxlength="18" password />
			</view>
			<view class="login-btn">
				登录
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				logo: "",
				account: "",
				password: "",
			}
		},
		methods: {}
	}
</script>

<style lang="scss" scoped>
	.head {
		height: 30vh;
		display: flex;
		justify-content: flex-end;
		align-items: center;
		flex-direction: column;

		.logo {
			width: 180rpx;
			height: 180rpx;
		}
	}

	.form-box {
		padding: 60rpx 80rpx;
		height: 100%;

		.row-input {
			margin-top: 50rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;
			color: #d1ccc3;
			height: 100rpx;
			font-size: 30rpx;
			border-bottom: 1rpx solid #bababa;

			image {
				padding: 0rpx 40rpx;
				width: 40rpx;
				height: 40rpx;
			}

			.input-placeholder {
				font-size: 28rpx;
				color: #adadad;
			}
		}

		.login-btn {
			margin-top: 80rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			color: #fff;
			width: 600rpx;
			height: 100rpx;
			font-size: 32rpx;
			border-radius: 10rpx;
			background-color: #5c70e2;
		}
	}
</style>