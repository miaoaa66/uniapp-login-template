<template>
	<view class="content">
		<view class="title">登录系统</view>
		<view class="sub-title">一旦用户成功登录，系统会创建一个会话，直到会话超时或用户主动登出。</view>
		<view class="form-box">
			<view class="row-input">
				<input maxlength="16" placeholder="用户ID" />
			</view>
			<view class="row-input">
				<input maxlength="16" placeholder="密码" password />
			</view>
			<view class="login-btn">登录</view>
			<view class="menu-link">
				<text @click="gotoRegister">新用户注册</text>
				<text @click="gotoForget">忘记密码?</text>
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
			gotoRegister() {
				uni.redirectTo({
					url: "/pages/login60/register"
				})
			},
			gotoForget() {
				uni.redirectTo({
					url: "/pages/login60/forget"
				})
			},
		}
	}
</script>

<style lang="scss" scoped>
	.title {
		font-size: 50rpx;
		font-weight: bold;
		padding: 60rpx 0 0 40rpx;
	}

	.sub-title {
		font-size: 30rpx;
		letter-spacing: 2rpx;
		line-height: 50rpx;
		padding: 20rpx 40rpx 0 40rpx;
	}

	.form-box {
		padding: 80rpx 40rpx 0rpx 40rpx;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 100rpx;
			margin: 40rpx 0rpx;
			padding: 0 20rpx;
			border-radius: 10rpx;
			border: 1rpx solid #e6e6e6;

			input {
				flex: 1;
			}
		}

		.login-btn {
			height: 100rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 32rpx;
			letter-spacing: 5rpx;
			color: #fff;
			background-color: #ffc605;
		}

		.menu-link {
			display: flex;
			justify-content: space-between;
			height: 100rpx;
			align-items: center;
			font-size: 26rpx;
			color: #ffc605;
		}
	}
</style>