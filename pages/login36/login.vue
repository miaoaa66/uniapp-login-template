<template>
	<view class="content">
		<view class="upper"></view>
		<view class="logo">
			<image mode="aspectFill"
				src="./static/login36_logo.png">
			</image>
		</view>
		<view class="form-box">
			<view class="top-switch">
				<view :class="[isLogin ? 'active' : '','login']" @click="switchClick(1)">登录</view>
				<view :class="[isLogin ? '' : 'active','register']" @click="switchClick(2)">注册</view>
			</view>
			<view class="row-input">
				<image mode="aspectFill"
					src="./static/login36_account.png">
				</image>
				<input type="text" placeholder="请输入用户名/手机号" maxlength="18" />
			</view>
			<view class="row-input">
				<image mode="aspectFill"
					src="./static/login36_password.png">
				</image>
				<input type="text" placeholder="请输入密码" maxlength="6" />
			</view>
			<view class="link">
				<text>忘记密码</text>
			</view>
			<view class="btn-box">
				<view class="btn login">{{isLogin?'登录':'注册'}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isLogin: true,
			}
		},
		methods: {
			// 切换
			switchClick(type) {
				this.isLogin = type == 1 ? true : false
			},
		}
	}
</script>

<style lang="scss" scoped>
	.upper {
		width: 100%;
		height: 40vh;
		background-color: #859ef8;
	}

	.logo {
		position: fixed;
		display: flex;
		justify-content: center;
		align-items: center;
		top: 10vh;
		left: 300rpx;

		image {
			width: 200rpx;
			height: 200rpx;
		}
	}

	.form-box {
		position: fixed;
		top: 30vh;
		left: 50rpx;
		right: 50rpx;
		border-radius: 10rpx;
		background-color: #FFFFFF;
		box-shadow: 0rpx 10rpx 15rpx #859ef8;

		.top-switch {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 120rpx;
			background-color: #a6a6a6;

			.login,
			.register {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 50%;
				height: 100%;
				border-top-left-radius: 10rpx;
				border-top-right-radius: 10rpx;
				background-color: #8ea6fc;
				color: #ffffff;
			}

			.active {
				background-color: #ffffff;
				color: #8ea6fc;
			}
		}

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			margin: 0rpx 60rpx;
			height: 140rpx;
			font-size: 30rpx;
			border-bottom: 1rpx solid #d8daff;

			input {
				padding: 0 20rpx;
			}

			image {
				width: 40rpx;
				height: 40rpx;
			}
		}

		.link {
			display: flex;
			justify-content: flex-end;
			align-items: center;
			height: 80rpx;
			margin: 0rpx 60rpx;
			font-size: 26rpx;
			color: #a6a6a6;
		}

		.btn-box {
			margin: 60rpx;

			.btn {
				margin: 40rpx 0rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				width: 100%;
				height: 100rpx;
				border-radius: 50rpx;
				font-size: 35rpx;
				letter-spacing: 5rpx;
			}

			.login {
				color: #FFFFFF;
				background: linear-gradient(to right, #8ea6fc, #758ff0)
			}
		}

	}
</style>