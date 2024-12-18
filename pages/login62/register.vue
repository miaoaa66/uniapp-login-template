<template>
	<view class="content">
		<view class="bg-left"></view>
		<view class="bg-right"></view>
		<view class="form-box">
			<view class="title">欢迎使用</view>
			<view class="input-box">
				<input placeholder="邮箱" maxlength="12" />
			</view>
			<view class="input-box">
				<input password placeholder="密码" maxlength="18" />
			</view>
			<view class="input-box">
				<input password placeholder="确认密码" maxlength="18" />
			</view>
			<view class="login-btn">注册</view>
			<view class="menu-link" @click="gotoLogin">去登录?</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		methods: {
			gotoLogin() {
				uni.redirectTo({
					url: "/pages/login62/login"
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
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