<template>
	<view class="content">
		<view class="row-input">
			<image mode="aspectFit" src="./static/login1_password.png"></image>
			<input placeholder="旧密码" maxlength="18" />
		</view>
		<view class="row-input">
			<image mode="aspectFit" src="./static/login1_password.png"></image>
			<input placeholder="新密码" maxlength="18" />
		</view>
		<view class="row-input">
			<image mode="aspectFit" src="./static/login1_password.png"></image>
			<input placeholder="再次新密码" maxlength="18" />
		</view>
		<view class="login-btn">
			立即修改
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.content {
		padding: 100rpx 60rpx;
		display: flex;
		flex-direction: column;
	}

	.row-input {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		padding-top: 30rpx;
		height: 100rpx;
		border-bottom: 1rpx solid #818181;

		image {
			width: 45rpx;
			height: 45rpx;
		}

		input {
			font-size: 30rpx;
			padding-left: 30rpx;
			width: 500rpx;
		}
	}

	.login-btn {
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 10rpx;
		margin-top: 80rpx;
		height: 80rpx;
		font-size: 30rpx;
		background-color: #fd661f;
		color: #FFFFFF;
	}
</style>
