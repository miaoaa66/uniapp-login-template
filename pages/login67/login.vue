<template>
	<view class="content">
		<view class="title">SIGN IN</view>
		<view class="row-input left-input">
			<image src="./static/login67_account.png">
			</image>
			<input placeholder="Email Address" maxlength="16" />
		</view>
		<view class="row-input right-input">
			<input placeholder="Password" password maxlength="18" />
			<image src="./static/login67_password.png">
			</image>
		</view>
		<view class="login-btn">登录</view>
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
		methods: {}
	}
</script>

<style lang="scss">
	page {
		background-color: #f2dc4f;
	}

	.title {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 300rpx;
		font-size: 70rpx;
		letter-spacing: 4rpx;
		font-weight: bold;
	}

	.row-input {
		display: flex;
		align-items: center;
		margin: 100rpx 0;
		width: 650rpx;
		height: 160rpx;
		font-size: 30rpx;
		background-color: #f8e9aa;

		image {
			margin: 0 40rpx;
			flex-shrink: 0;
			width: 60rpx;
			height: 60rpx;
		}

		input {
			padding-left: 30rpx;
			font-size: 30rpx;
			flex: 1;
		}
	}

	.left-input {
		margin-left: 100rpx;
		border-top-left-radius: 100rpx;
		border-bottom-left-radius: 100rpx;
	}


	.right-input {
		margin-right: 100rpx;
		border-top-right-radius: 100rpx;
		border-bottom-right-radius: 100rpx;
	}

	.login-btn {
		margin: auto;
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 100%;
		width: 300rpx;
		height: 300rpx;
		font-size: 60rpx;
		font-weight: bold;
		color: #ccb842;
		background-color: #f8e9aa;
	}
</style>