<template>
	<view class="content">
		<image class="bg" src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login19/login19_bg.png"></image>
		<view class="title">XX系统</view>
		<view class="form-box">
			<text class="name">登录</text>
			<view class="row-input" style="border-bottom: 2rpx solid #e9deec;">
				<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login19/login19_account.png"></image>
				<input placeholder="请输入账号" maxlength="12" />
			</view>
			<view class="row-input">
				<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login19/login19_password.png"></image>
				<input placeholder="请输入密码" maxlength="12" password />
			</view>
			<view class="menu-link">
				<text>注册</text>
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

<style lang="scss">
	.bg {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: -1;
		width: 100%;
		height: 100%;
	}

	.title {
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 50rpx;
		letter-spacing: 10rpx;
		font-weight: bold;
		height: 18vh;
	}

	.form-box {
		position: fixed;
		padding: 100rpx 80rpx;
		top: 16vh;
		left: 40rpx;
		right: 40rpx;
		border-radius: 20rpx;
		background-color: #FFFFFF;

		.name {
			line-height: 120rpx;
			font-size: 50rpx;
			letter-spacing: 5rpx;
			font-weight: bold;
			color: #7531f7;
		}

		.row-input {
			height: 150rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;

			image {
				width: 40rpx;
				height: 40rpx;
				flex-shrink: 0;
			}

			input {
				padding-left: 40rpx;
			}
		}

		.menu-link {
			display: flex;
			justify-content: space-between;
			line-height: 100rpx;
			font-size: 24rpx;
			color: #a5a5a5;
		}

		.login-btn {
			margin-top: 50rpx;
			height: 90rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			color: #FFFFFF;
			font-size: 30rpx;
			border-radius: 50rpx;
			background-color: #6722ee;
		}
	}
</style>
