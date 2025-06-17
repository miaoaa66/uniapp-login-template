<template>
	<view class="content">
		<view class="head">
			<view class="logo">
				<text>HELLO,</text>
				<text>欢迎使用</text>
			</view>
			<view class="tip">数据采集</view>
		</view>
		<view class="form-box">
			<view class="tab-box">
				<view class="container left-style" :class="tabIndex==0?'active':'no-active'" @click="tabClick(0)">
					<view class="left-parallelogram" :class="tabIndex==0?'parallelogram':'no-parallelogram'"></view>
					<view class="text" style="left: 20rpx; ">手机快速登录</view>
				</view>
				<view class="container right-style" :class="tabIndex==1?'active':'no-active'" @click="tabClick(1)">
					<view class="right-parallelogram" :class="tabIndex==1?'parallelogram':'no-parallelogram'"></view>
					<view class="text" style="left: -10rpx;">账号密码登录</view>
				</view>
			</view>
			<view v-show="tabIndex==0">
				<view class="row-input">
					<image src="./static/login85_phone.png"></image>
					<input maxlength="11" placeholder="请输入手机号码" />
				</view>
				<view class="row-input">
					<image src="./static/login85_code.png"></image>
					<input maxlength="6" placeholder="请输入验证码" />
					<view class="code">
						<text @click="getCode">获取验证码</text>
					</view>
				</view>
			</view>
			<view v-show="tabIndex==1">
				<view class="row-input">
					<image src="./static/login85_phone.png"></image>
					<input maxlength="11" placeholder="请输入手机号码" />
				</view>
				<view class="row-input">
					<image src="./static/login85_password.png"></image>
					<input password maxlength="18" placeholder="请输入密码" />
				</view>
			</view>
			<view class="login-btn">
				立即登录
			</view>
			<view class="menu-link">
				<text @click="register">没有账号？</text>
				<text style="color: #62ae63;">去注册</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabIndex: 0, //tab下标
				code: "",
				password: "",
				phone: "",
			}
		},
		onLoad() {
		},
		methods: {
			// tab点击
			tabClick(index) {
				this.tabIndex = index;
			},

			// 获取验证码
			getCode() {
				uni.showToast({
					title: "获取验证码"
				})
			},
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-image: linear-gradient(90deg, #e9efc8, #cff1c9);
	}

	.head {
		padding-left: 80rpx;
		height: 30vh;
		display: flex;
		justify-content: flex-start;
		align-items: center;


		.logo {
			display: flex;
			flex-direction: column;
			align-items: flex-start;
			font-size: 40rpx;
			font-weight: bold;
		}

		.tip {
			padding: 10rpx 20rpx;
			margin-top: 50rpx;
			margin-left: 20rpx;
			color: #FFFFFF;
			font-size: 30rpx;
			letter-spacing: 4rpx;
			font-weight: bold;
			background-color: #62ae63;
			border-top-right-radius: 50rpx;
			border-top-left-radius: 50rpx;
			border-bottom-right-radius: 50rpx;
		}
	}

	.form-box {
		padding: 0px 80rpx;
		height: 70vh;
		border-top-left-radius: 80rpx;
		border-top-right-radius: 80rpx;
		background-color: #FFFFFF;

		.tab-box {
			padding-top: 60rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			height: 100rpx;

			.container {
				position: relative;
				width: 240rpx;
				height: 100%;
			}

			.parallelogram {
				position: absolute;
				height: 100%;
				background-color: #62ae63;
				transform-origin: bottom;
			}

			.no-parallelogram {
				position: absolute;
				height: 100%;
				background-color: #eff9f1;
				transform-origin: bottom;
			}

			.text {
				position: absolute;
				top: 0;
				width: 100%;
				height: 100%;
				display: flex;
				justify-content: center;
				align-items: center;
				letter-spacing: 2rpx;
				font-size: 30rpx;
				z-index: 1;
			}

			.left-style {
				border-radius: 60rpx 0 0 60rpx;
			}

			.left-parallelogram {
				right: -10rpx;
				width: 100rpx;
				transform: skew(-30deg);
				border-top-right-radius: 30rpx;
				border-bottom-right-radius: 10rpx;
			}

			.right-style {
				border-radius: 0 60rpx 60rpx 0;
			}

			.right-parallelogram {
				left: -70rpx;
				width: 100rpx;
				transform: skew(-30deg);
				border-top-left-radius: 10rpx;
				border-bottom-left-radius: 30rpx;
			}

			.active {
				background-color: #62ae63;
				color: #fff;
			}

			.no-active {
				background-color: #eff9f1;
				color: #62ae63;
			}
		}


		.row-input {
			display: flex;
			justify-content: space-between;
			align-items: center;
			margin: 30rpx 0;
			font-size: 30rpx;
			height: 100rpx;
			border-radius: 100rpx;
			background-color: #f7f7f7;

			image {
				margin: 0 20rpx;
				width: 35rpx;
				height: 35rpx;
			}

			input {
				flex: 1;
			}

			.code {
				display: flex;
				justify-content: center;
				font-weight: bold;
				color: #62ae63;
				width: 200rpx;
				font-size: 26rpx;
			}
		}

		.login-btn {
			margin-top: 60rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			height: 100rpx;
			letter-spacing: 10rpx;
			background-color: #62ae63;
			color: #FFFFFF;
			border-radius: 100rpx;
			font-size: 30rpx;
		}

		.menu-link {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 100rpx;
			font-size: 28rpx;
		}
	}
</style>