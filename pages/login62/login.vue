<template>
	<view class="content">
		<view class="bg-left"></view>
		<view class="bg-right"></view>
		<view class="form-box">
			<view class="title">欢迎回来!</view>
			<view class="input-box">
				<text class="name">邮箱地址</text>
				<input placeholder="请输入邮箱" maxlength="12" />
			</view>
			<view class="input-box">
				<text class="name">密码</text>
				<input password placeholder="请输入密码" maxlength="18" />
			</view>
			<view class="login-btn">登录</view>
			<view class="menu-link" @click="gotoRegister">没有账号?</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		methods: {
			gotoRegister() {
				uni.redirectTo({
					url: "/pages/login62/register"
				})
			}
		}
	}
</script>

<style lang="scss">
	.bg-left {
		position: fixed;
		right: -320rpx;
		top: -500rpx;
		width: 500rpx;
		height: 500rpx;
		border: 100rpx solid #f8dcbc;
		transform: rotate(60deg);
		z-index: -1;
	}

	.bg-right {
		position: fixed;
		bottom: -320rpx;
		left: -320rpx;
		width: 500rpx;
		height: 500rpx;
		border: 100rpx solid #f6dae8;
		border-radius: 100%;
		transform: rotate(40deg);
		z-index: -1;
	}


	.form-box {
		margin: 200rpx 80rpx 0 80rpx;

		.title {
			margin-bottom: 100rpx;
			font-size: 60rpx;
			font-weight: bold;
		}

		.input-box {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: flex-start;
			margin: 20rpx 0;
			padding: 10rpx 20rpx;
			font-size: 30rpx;
			height: 100rpx;
			border-radius: 10rpx;
			background-color: #f7f6fb;
			color: #000;

			.name {
				height: 40rpx;
			}

			input {
				font-size: 30rpx;
				flex: 1;
				color: #000;
			}
		}

		.login-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			margin: 60rpx auto;
			width: 500rpx;
			height: 100rpx;
			border-radius: 50rpx;
			font-size: 32rpx;
			letter-spacing: 4rpx;
			color: #ffffff;
			background-color: #5abdb5;
		}

		.menu-link {
			display: flex;
			justify-content: center;
			align-items: center;
			margin: auto;
			font-size: 30rpx;
			color: #ffaa00;
		}
	}
</style>