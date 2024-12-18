<template>
	<view class="content">
		<image mode="widthFix" src="./static/login30_bg.png"></image>
		<view class="logo">LOGO</view>
		<view class="form-box">
			<text class="title">Welcome</text>
			<view class="row-input">
				<input placeholder="用户名" maxlength="12" />
			</view>
			<view class="row-input">
				<input placeholder="密码" maxlength="18" />
			</view>
			<view class="menu-link">
				<text>忘记密码?</text>
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

<style lang="scss" scoped>
	image {
		width: 100%;
		height: 100%;
	}

	.logo {
		display: flex;
		justify-content: center;
		align-items: center;
		font-weight: bold;
		font-size: 40rpx;
		color: #81d7cf;
		position: fixed;
		top: 200rpx;
		left: 300rpx;
		width: 150rpx;
		height: 150rpx;
		background-color: #FFFFFF;
		border-radius: 100%;
	}

	.form-box {
		position: fixed;
		top: 450rpx;
		right: 150rpx;
		left: 150rpx;


		.title {
			font-size: 50rpx;
			font-weight: bold;
			letter-spacing: 10rpx;
			display: flex;
			justify-content: center;
			align-items: center;
		}

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: flex-end;
			height: 120rpx;
			border-bottom: 1rpx solid #c9e6e3;

			input {
				flex: 1;
				font-size: 30rpx;
			}
		}

		.menu-link {
			display: flex;
			justify-content: flex-end;
			align-items: center;
			height: 50rpx;
			font-size: 26rpx;
			color: #81d7cf;
		}

		.login-btn {
			margin-top: 50rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			height: 80rpx;
			width: 100%;
			font-size: 32rpx;
			background-color: #81d7cf;
			color: #FFFFFF;
			border-radius: 30rpx;
		}
	}
</style>
