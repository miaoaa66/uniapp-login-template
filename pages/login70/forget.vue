<template>
	<view class="content">
		<view class="title">
			设置密码
		</view>
		<view class="row-input">
			<input placeholder="请输入密码" maxlength="18" />
		</view>
		<view class="row-input">
			<input placeholder="再次输入密码" maxlength="18" />
		</view>
		<view class="login-btn">确认</view>
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

		input {
			width: 480rpx;
			font-size: 30rpx;
		}
	}

	.login-btn {
		margin-top: 100rpx;
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