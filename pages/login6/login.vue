<template>
	<view class="content">
		<view class="head">
			<image mode="aspectFit" src="./static/login6_logo.png"></image>
		</view>
		<view class="form-box">
			<view class="row-input">
				<input placeholder="手机号" maxlength="11" type="number" />
			</view>
			<view class="row-input">
				<input placeholder="密码" password maxlength="18" />
			</view>
			<view class="login-btn">登录</view>
			<view class="menu-link">
				<text>忘记密码</text>
				<text @click="goToRegister">注册账号</text>
			</view>
			<view class="line">第三方账号登录</view>
			<view class="quick-login">
				<image src="./static/login6_vx.png"></image>
				<image src="./static/login6_qq.png"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		methods: {
			// 去注册
			goToRegister() {
				uni.navigateTo({
					url: '/pages/login6/register'
				})
			}
		}
	}
</script>

<style lang="scss">
	.head {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 32vh;

		image {
			width: 360rpx;
			height: 360rpx;
		}
	}

	.form-box {
		padding: 0rpx 80rpx;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 120rpx;
			border-bottom: 1rpx solid #ececec;
		}

		.login-btn {
			margin-top: 30rpx;
			width: 100%;
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 30rpx;
			height: 100rpx;
			background-color: #393a3e;
			letter-spacing: 10rpx;
			color: #f1f1f3;
		}

		.menu-link {
			line-height: 100rpx;
			font-size: 28rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			color: #676769;
		}

		.line {
			width: 100%;
			margin: 50rpx 0rpx;
			align-self: center;
			position: relative;
			display: flex;
			flex-direction: row;
			justify-content: center;
			align-items: center;
		}

		.line:before {
			content: "";
			margin-right: 20rpx;
			border-top: 1px solid #dddddd;
			flex: 1 1 0rpx;
		}

		.line::after {
			content: "";
			margin-left: 20rpx;
			border-top: 1px solid #dddddd;
			flex: 1 1 0rpx;
		}


		.quick-login {
			padding: 0 120rpx;
			display: flex;
			justify-content: space-around;
			align-items: center;

			image {
				width: 100rpx;
				height: 100rpx;
				border-radius: 100%;
				background-color: #000000;
			}
		}
	}
</style>
