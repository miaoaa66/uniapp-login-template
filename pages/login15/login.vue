<template>
	<view class="content">
		<image class="bg" mode="heightFix" src="./static/login15_bg.png"></image>
		<view class="logo">LOGO</view>
		<view class="form-box">
			<view class="row-input">
				<input placeholder="用户名" maxlength="12" placeholder-class="placeholder"/>
			</view>
			<view class="row-input">
				<input placeholder="密码" password maxlength="18" placeholder-class="placeholder"/>
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
	.bg {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: -1;
		height: 100%;
		width: 100%;
	}

	.logo {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 35vh;
		font-size: 60rpx;
		color: #FFFFFF;
		letter-spacing: 10rpx;
	}

	.form-box {
		margin-top: 12vh;
		padding: 0rpx 80rpx;

		.row-input {
			display: flex;
			align-items: center;
			padding: 0rpx 50rpx;
			margin: 50rpx 0rpx;
			border-radius: 50rpx;
			height: 100rpx;
			border: 1rpx solid #d4c4fd;
		}
		
		.placeholder{
			color: #8971f8;
			font-size: 30rpx;
		}
		
		input{
			width: 100%;
			color: #8971f8;
			font-size: 30rpx;
		}

		.login-btn {
			margin-top: 80rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			letter-spacing: 30rpx;
			height: 100rpx;
			border-radius: 50rpx;
			color: #e0cffa;
			font-size: 35rpx;
			background: linear-gradient(#a563f8, #7f72f6);
		}
	}
</style>
