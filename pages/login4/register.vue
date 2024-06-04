<template>
	<view class="content">
		<view class="row-input">
			<text>电话号码</text>
			<view class="input-box">
				<input placeholder="请输入您的电话号码" maxlength="11" />
			</view>
		</view>
		<view class="row-input2">
			<text>验证码</text>
			<view class="input-box2">
				<input class="input2" placeholder="请输入您的验证码" maxlength="6" />
				<view class="code">
					<text>获取验证码</text>
				</view>
			</view>
		</view>
		<view class="row-input">
			<text>登录密码</text>
			<view class="input-box">
				<input placeholder="请输入您的登录密码" maxlength="18" />
			</view>
		</view>
		<view class="row-input">
			<text>确认密码</text>
			<view class="input-box">
				<input placeholder="再次输入登录密码" maxlength="18" />
			</view>
		</view>
		<view class="row-input">
			<text>邀请码</text>
			<view class="input-box">
				<input placeholder="请输入您的邀请码" maxlength="10" />
			</view>
		</view>
		<view class="register-btn">
			注册
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
	.content {
		padding: 0 40rpx;

		.row-input {
			display: flex;
			flex-direction: column;
			font-size: 30rpx;

			text {
				font-size: 28rpx;
				line-height: 100rpx;
				font-weight: bold;
			}

			.input-box {
				display: flex;
				justify-content: space-between;
				align-items: center;
				padding: 0 20rpx;
				height: 80rpx;
				border-radius: 8rpx;
				background-color: #f2f3f4;

				.code {
					display: flex;
					justify-content: center;
					align-items: center;
					font-size: 28rpx;
					letter-spacing: 3rpx;
					width: 200rpx;
					height: 80rpx;
					border-radius: 8rpx;
					background-color: #0a7ff7;
					color: #FFFFFF;
				}
			}
		}


		.row-input2 {
			display: flex;
			flex-direction: column;
			font-size: 30rpx;

			text {
				font-size: 28rpx;
				line-height: 100rpx;
				font-weight: bold;
			}

			.input-box2 {
				display: flex;
				justify-content: space-between;
				align-items: center;
				
				.input2{
					padding: 0 20rpx;
					width: 400rpx;
					height: 80rpx;
					border-radius: 8rpx;
					background-color: #f2f3f4;
				}

				.code {
					display: flex;
					justify-content: center;
					align-items: center;
					font-size: 28rpx;
					letter-spacing: 3rpx;
					width: 200rpx;
					height: 80rpx;
					border-radius: 8rpx;
					background-color: #0a7ff7;
					color: #a5ceff;
				}
			}
		}

		.register-btn {
			margin-top: 60rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 100rpx;
			background: linear-gradient(to right, #0a7df7, #0f62f3);
			color: #FFFFFF;
			letter-spacing: 10rpx;
			font-size: 30rpx;
			border-radius: 10rpx;
		}
	}
</style>
