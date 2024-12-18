<template>
	<view class="content">
		<view class="head">
			<text>LOGO</text>
		</view>
		<view class="form-box">
			<image class="bg-set" src="./static/login5_login_bg.png"></image>
			<view class="form">
				<text class="tip">注册</text>
				<view class="row-input">
					<view class="row-img">
						<image mode="aspectFit" src="./static/login5_account.png"></image>
					</view>
					<input placeholder="昵称" maxlength="12" />
					<text class="row-lable" style="background-color: #551ead;">用户名</text>
				</view>
				<view class="row-input">
					<view class="row-img">
						<image mode="aspectFit" src="./static/login5_email.png"></image>
					</view>
					<input placeholder="your email@xxxx.com" maxlength="18" />
					<text class="row-lable" style="background-color: #5a27b7;">邮箱</text>
				</view>
				<view class="row-input">
					<view class="row-img">
						<image mode="aspectFit" src="./static/login5_phone.png"></image>
					</view>
					<input placeholder="+86" maxlength="11" />
					<text class="row-lable" style="background-color: #5d2cbc;">手机</text>
				</view>
				<view class="row-input">
					<view class="row-img">
						<image mode="aspectFit" src="./static/login5_password.png"></image>
					</view>
					<input placeholder="密码" maxlength="12" />
					<text class="row-lable" style="background-color: #6133c3;">密码</text>
				</view>
				<view class="check-box">
					<checkbox :checked="true" style="transform: scale(0.7);" />
					<text>是的! 我同意用户协议</text>
				</view>
				<view class="login-btn">注册</view>
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
	.content {
		display: flex;
		flex-direction: column;
		width: 100%;
		height: 100vh;
		color: #007AFF;

		.head {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 30vh;

			text {
				display: flex;
				justify-content: center;
				align-items: center;
				letter-spacing: 8rpx;
				width: 140rpx;
				height: 140rpx;
				border-radius: 50%;
				background-color: #541eab;
				color: #FFFFFF;
			}
		}

		.form-box {
			position: relative;
			display: flex;
			align-items: center;
			flex-direction: column;
			height: 70vh;

			.bg-set {
				position: absolute;
				width: 100%;
				height: 100%;
				z-index: 1;
			}

			.form {
				z-index: 2;
				position: absolute;

				.tip {
					display: flex;
					justify-content: center;
					padding: 60rpx 0rpx 30rpx 0rpx;
					font-size: 60rpx;
					color: #FFFFFF;
					letter-spacing: 20rpx;
				}

				.row-input {
					position: relative;
					margin: 30rpx 0;
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
						color: #aeb0b9;
					}

					input {
						color: #e8e8e8;
					}
				}

				.check-box {
					width: 100%;
					display: flex;
					justify-content: flex-start;
					align-items: center;
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
					font-size: 30rpx;
					background-color: #FFFFFF;
					color: #613abd;
					letter-spacing: 5rpx;
					font-weight: bold;
				}
			}
		}
	}
</style>
