<template>
	<view class="content">
		<view class="head">
			<image class="bg-set" src="./static/login5_register_bg.png"></image>
		</view>
		<view class="form-box">
			<text class="tip">登录</text>
			<view class="row-input">
				<view class="row-img">
					<image mode="aspectFit" src="./static/login5_email.png"></image>
				</view>
				<input placeholder="your email@xxxx.com" maxlength="18" />
				<text class="row-lable" style="background-color: #FFFFFF;">邮箱</text>
			</view>
			<view class="row-input">
				<view class="row-img">
					<image mode="aspectFit" src="./static/login5_password.png"></image>
				</view>
				<input placeholder="密码" maxlength="18" password />
				<text class="row-lable" style="background-color: #FFFFFF;">密码</text>
			</view>
			<view class="login-btn">登录</view>
			<text class="menu-link">忘记密码?</text>
			<view class="register-tip">
				还没有账号？<text @click="gotoRegister">立即注册</text>
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
			gotoRegister() {
				uni.navigateTo({
					url: '/pages/login5/register'
				})
			}
		}
	}
</script>

<style lang="scss">
	.content {
		display: flex;
		flex-direction: column;
		width: 100%;
		height: 100vh;

		.head {
			width: 100%;
			height: 40vh;

			.bg-set {
				width: 100%;
				height: 100%;
			}
		}


		.form-box {
			display: flex;
			align-items: center;
			flex-direction: column;
			position: fixed;
			top: 35vh;
			left: 0;
			right: 0;
			bottom: 0;

			.tip {
				padding-bottom: 20rpx;
				font-size: 60rpx;
				letter-spacing: 20rpx;
			}

			.row-input {
				margin: 25rpx 0;
				position: relative;
				display: flex;
				justify-content: flex-start;
				align-items: center;
				width: 600rpx;
				height: 100rpx;
				border: 4rpx solid #e2e2e2;
				border-radius: 70rpx;

				.row-img {
					margin-right: 30rpx;
					padding-left: 10rpx;
					width: 100rpx;
					height: 60rpx;
					display: flex;
					justify-content: center;
					align-items: center;
					border-right: 4rpx solid #e2e2e2;

					image {
						width: 50rpx;
						height: 50rpx;
					}
				}

				.row-lable {
					position: absolute;
					width: 100rpx;
					display: block;
					left: 100rpx;
					top: -20rpx;
					font-size: 26rpx;
					text-align: center;
					color: #c5c7d1;
				}

				input {
					color: #c3c3c3;
				}
			}

			.check-box {
				width: 100%;
				padding-left: 180rpx;
				display: flex;
				justify-content: flex-start;
				align-items: flex-end;
				color: #dfdaef;
				font-size: 26rpx;
				letter-spacing: 5rpx;
			}

			.login-btn {
				margin-top: 30rpx;
				width: 600rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				height: 100rpx;
				border-radius: 50rpx;
				font-size: 40rpx;
				background: linear-gradient(#501da2, #6b47d6);
				letter-spacing: 20rpx;
				color: #FFFFFF;
			}

			.menu-link {
				line-height: 80rpx;
				letter-spacing: 3rpx;
				color: #5621af;
				font-size: 26rpx;
			}

			.register-tip {
				position: fixed;
				color: #adadad;
				bottom: 30rpx;
				font-size: 28rpx;

				text {
					color: #5621af;
				}
			}
		}
	}
</style>
