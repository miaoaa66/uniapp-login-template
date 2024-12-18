<template>
	<view class="content">
		<view class="bg-form"></view>
		<view class="form-box">
			<view class="title">登录</view>
			<view class="input-box">
				<image src="@/static/logo.png">
				</image>
				<input placeholder="请输入用户名" maxlength="12" />
			</view>
			<view class="input-box">
				<image src="@/static/logo.png">
				</image>
				<input password placeholder="请输入密码" maxlength="18" />
			</view>
			<view class="login-btn">现在登录</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				account: "",
				password: "",
			}
		},
		onLoad() {
		},
		methods: {}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #424444;
	}

	.bg-form {
		position: fixed;
		top: 100rpx;
		left: -500rpx;
		width: 1000rpx;
		height: 1000rpx;
		border-radius: 100rpx;
		background-color: #fbfbfd;
		transform: rotate(140deg);
		z-index: 2;
	}

	.form-box {
		position: absolute;
		z-index: 3;
		margin-top: 200rpx;
		margin-left: 60rpx;
		width: 420rpx;

		.title {
			height: 200rpx;
			color: #424444;
			font-size: 60rpx;
			font-weight: bold;
		}

		.input-box {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 120rpx;
			border-bottom: 1rpx solid #b1b6b6;

			image {
				width: 30rpx;
				height: 30rpx;
				padding: 0 20rpx;
			}

			input {
				font-size: 30rpx;
				flex: 1;
			}
		}

		.login-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			margin: 100rpx 100rpx 0 100rpx;
			width: 450rpx;
			height: 100rpx;
			font-size: 34rpx;
			border-radius: 50rpx;
			letter-spacing: 4rpx;
			color: #efdfda;
			background-color: #cb4949;
		}
	}
</style>