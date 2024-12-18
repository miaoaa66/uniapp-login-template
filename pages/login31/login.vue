<template>
	<view class="content">
		<view class="title">
			<text class="name">NIEW</text>
			<text class="sub-name">欢迎您登录系统~</text>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image class="icon"
					src="./static/login31_phone.png">
				</image>
				<input placeholder="请输入手机号码" maxlength="12" />
			</view>
			<view class="row-input">
				<image class="icon"
					src="./static/login31_password.png">
				</image>
				<input placeholder="请输入账号密码" :password="showPassword" maxlength="18" />
				<image class="sub-icon" v-if="!showPassword" @click="passwordChange"
					src="./static/login31_show.png">
				</image>
				<image class="sub-icon" mode="aspectFit" v-else @click="passwordChange"
					src="./static/login31_hide.png">
				</image>
			</view>
			<view class="menu-link">
				<text>忘记密码?</text>
			</view>
			<view class="login-btn">立即登录</view>
			<view class="tip-link">
				还没有账号?<text>立即注册</text>
			</view>
		</view>
		<view class="shortcut">
			<view class="tips">
				<text class="line"></text>
				其他方式登录
				<text class="line"></text>
			</view>
			<view class="icon-box">
				<image
					src="./static/login31_wx.png">
				</image>
				<image
					src="./static/login31_qq.png">
				</image>
				<image
					src="./static/login31_wb.png">
				</image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showPassword: false,
			}
		},
		methods: {

			// 密码图标切换
			passwordChange() {
				this.showPassword = !this.showPassword
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #FFFFFF;
	}

	.title {
		padding-left: 60rpx;
		display: flex;
		justify-content: center;
		align-items: flex-start;
		flex-direction: column;
		height: 20vh;

		.name {
			font-size: 70rpx;
			line-height: 100rpx;
			font-weight: bold;
		}

		.sub-name {
			line-height: 80rpx;
			font-size: 45rpx;
			letter-spacing: 8rpx;
			font-weight: bold;
		}
	}

	.form-box {
		padding: 0rpx 60rpx;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 100rpx;
			border-radius: 12rpx;
			margin: 50rpx 0rpx;

			.icon {
				margin-right: 20rpx;
				flex-shrink: 0;
				width: 35rpx;
				height: 35rpx;
			}

			input {
				font-size: 30rpx;
				flex: 1;
			}

			.sub-icon {
				margin-left: 20rpx;
				flex-shrink: 0;
				width: 35rpx;
				height: 35rpx;
			}
		}

		.menu-link {
			display: flex;
			justify-content: flex-end;
			align-items: center;
			color: #3750ff;
			font-size: 26rpx;
		}

		.login-btn {
			margin-top: 50rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			height: 90rpx;
			border-radius: 12rpx;
			background-color: #3750ff;
			color: #ffffff;
			font-size: 30rpx;
			letter-spacing: 5rpx;
		}

		.tip-link {
			display: flex;
			justify-content: center;
			letter-spacing: 3rpx;
			line-height: 150rpx;
			font-size: 26rpx;
			font-weight: bold;

			text {
				padding-left: 10rpx;
				color: #3750ff;
			}
		}
	}

	.shortcut {
		display: flex;
		flex-direction: column;

		.tips {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 100rpx;
			font-size: 26rpx;

			.line {
				width: 60rpx;
				height: 2rpx;
				margin: 0 40rpx;
				background-color: #dad9d9;
			}
		}

		.icon-box {
			height: 140rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			
			image{
				margin: 0 40rpx;
				width: 80rpx;
				height: 80rpx;
			}
		}
	}
</style>