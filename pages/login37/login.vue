<template>
	<view class="content">
		<image class="bg" mode="aspectFill" src="./static/login37_bg.png"></image>
		<view class="form-box">
			<view class="title">登录</view>
			<view class="row-input">
				<input type="text" placeholder="账号" maxlength="18" />
			</view>
			<view class="row-input">
				<input type="text" placeholder="密码" password="true" maxlength="16" />
			</view>
			<view class="btn-box">
				<view class="btn login">登录</view>
			</view>
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

<style lang="scss" scoped>
	
	.bg{
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: -1;
		width: 100%;
		height: 100%;
	}

	.form-box {
		position: fixed;
		background: inherit;
		top: 30vh;
		left: 50rpx;
		right: 50rpx;
		padding: 60rpx 60rpx 0rpx 60rpx;
		border-radius: 30rpx;
		background-color: rgba(255,255,255,.25) !important;
		
		.title{
			display: flex;
			justify-content: center;
			align-items: center;
			height: 100rpx;
			letter-spacing: 6rpx;
			font-size: 60rpx;
			color: #FFFFFF;
			font-weight: bold;
		}


		.row-input {
			display: flex;
			justify-content: space-between;
			align-items: center;
			margin: 40rpx 0rpx;
			height: 100rpx;
			font-size: 28rpx;
			padding: 0 20rpx;
			background-color: #FFFFFF;
			border-radius: 10rpx;
		}

		.btn-box {
			padding: 20rpx 0rpx 60rpx 0rpx;

			.btn {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 100%;
				height: 100rpx;
				border-radius: 10rpx;
				font-size: 35rpx;
				letter-spacing: 5rpx;
			}

			.login {
				color: #FFFFFF;
				background-color: #9faa9f;
			}
		}
	}

</style>