<template>
	<view class="content">
		<view class="title">重置密码</view>
		<view class="sub-title">重置验证码发送到您的电子邮件，请输入代码并创建新密码。</view>
		<view class="form-box">
			<view class="row-input">
				<input maxlength="16" placeholder="验证码" />
			</view>
			<view class="row-input">
				<input maxlength="16" placeholder="新的密码" password />
			</view>
			<view class="row-input">
				<input maxlength="16" placeholder="确认密码" password />
			</view>
			<view class="login-btn">更改密码</view>
			<view class="menu-link">
				<text @click="gotoRegister">新用户注册</text>
				<text @click="gotoLogin">有账号?去登录</text>
				
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
			gotoLogin() {
				uni.redirectTo({
					url: "/pages/login60/login"
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