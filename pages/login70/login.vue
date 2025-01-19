<template>
	<view class="content">
		<view class="title">
			欢迎回来
		</view>
		<view class="row-input">
			<input placeholder="请输入用户名" maxlength="16" />
		</view>
		<view class="row-input">
			<input placeholder="请输入密码" maxlength="18" password />
		</view>
		<view class="menu-link">
			<text @click="forget">忘记密码</text>
			<text @click="register">注册账号</text>
		</view>
		<view class="login-btn">登录</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		methods: {
			// 注册
			register() {
				uni.navigateTo({
					url: '/pages/login70/register'
				})
			},

			// 忘记密码
			forget() {
				uni.navigateTo({
					url: '/pages/login70/forget'
				})
			}
		}
	}
</script>

<style lang="scss">
	.content {
		padding: 0 60rpx;
	}

	.title {
		height: 300rpx;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		font-size: 60rpx;
		font-weight: bold;
		letter-spacing: 5rpx;
		color: #000000;
	}

	.row-input {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		height: 160rpx;
		border-bottom: 2rpx solid #b1b1b1;

		image {
			margin: 0 20rpx;
			flex-shrink: 0;
			width: 36rpx;
			height: 40rpx;
		}

		input {
			width: 480rpx;
			font-size: 30rpx;
		}
	}

	.menu-link {
		display: flex;
		justify-content: space-between;
		line-height: 100rpx;
		font-size: 30rpx;
		color: #000000;
		padding-bottom: 20rpx;
	}

	.login-btn {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		color: #FFFFFF;
		font-size: 32rpx;
		letter-spacing: 20rpx;
		height: 100rpx;
		border-radius: 50rpx;
		background-color: #0f71e3;
	}
</style>