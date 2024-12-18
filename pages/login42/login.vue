<template>
	<view class="content">
		<view class="head">LOGO</view>
		<view class="form-box">
			<view class="row-input">
				<input placeholder="用户名" maxlength="12" />
			</view>
			<view class="row-input">
				<input placeholder="密码" password maxlength="18" />
			</view>
		</view>
		<view class="btn-box">
			<text class="login btn">登录</text>
			<text class="register btn">注册</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		methods: {}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #222e3c;
	}

	.head {
		display: flex;
		justify-content: center;
		align-items: flex-end;
		width: 100%;
		height: 25vh;
		font-size: 80rpx;
		font-weight: bold;
		letter-spacing: 2rpx;
		color: #9ec552;
	}

	.form-box {
		margin: 80rpx 40rpx 20rpx 40rpx;
		border-radius: 6rpx;
		border: 1rpx solid #222427;
		box-sizing: border-box;
		background-color: #FFFFFF;

		.row-input {
			margin: 0 40rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;
			border-bottom: 1rpx solid #eaeaea;
			height: 120rpx;
			font-size: 30rpx;

			input {
				flex: 1;
				width: 400rpx;
				color: #818181;
			}
		}
	}

	.btn-box {
		margin: 20rpx 40rpx;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;

		.btn {
			display: flex;
			justify-content: center;
			align-items: center;
			margin: 10rpx 0;
			width: 100%;
			height: 100rpx;
			border-radius: 6rpx;
			font-size: 30rpx;
		}

		.login {
			background-color: #84bc3e;
			border: 2rpx solid #cccccc;
			box-sizing: border-box;
			color: #FFFFFF;
		}

		.register {
			background-color: #2f323a;
			border: 2rpx solid #121418;
			box-sizing: border-box;
			color: #cbd0d6;
		}
	}
</style>