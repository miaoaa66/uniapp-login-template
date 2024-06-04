<template>
	<view class="content">
		<view class="title">登录</view>
		<view class="form-box">
			<view class="row-input">
				<text>用户名</text>
				<input maxlength="12" />
			</view>
			<view class="row-input">
				<text>密码</text>
				<input maxlength="18" />
			</view>
			<view class="login-btn">登录</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
	}
</script>

<style lang="scss">
	.title {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		padding-left: 60rpx;
		letter-spacing: 10rpx;
		height: 20vh;
		font-weight: bold;
		font-size: 60rpx;
		color: #9a99f6;
	}

	.form-box {
		padding: 40rpx;

		.row-input {
			padding-top: 60rpx;
			display: flex;
			flex-direction: column;
			justify-content: center;
			height: 100rpx;
			border-bottom: 1rpx solid #c8c8c8;

			text {
				font-size: 30rpx;
				line-height: 40rpx;
				color: #bcbcbc;
			}

			input {
				flex: 1;
				font-size: 30rpx;
			}
		}

		.login-btn {
			margin-top: 80rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 40rpx;
			letter-spacing: 10rpx;
			font-weight: bold;
			color: #FFFFFF;
			background-image: linear-gradient(to right, #a1c4fc, #998cf5);
			box-shadow: 0rpx 5rpx 30rpx #cabdf5;
			height: 110rpx;
			border-radius: 50rpx;
			width: 100%;
		}

	}
</style>
