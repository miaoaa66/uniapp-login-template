<template>
	<view class="content">
		<view class="tip-box">
			<view class="tip">
				<text>登录</text>
				<text class="thing"></text>
			</view>
			<text class="sub-tip">还没有账号，立即注册</text>
		</view>
		<view class="row-input">
			<image mode="aspectFit"
				src="./static/login9_account.png">
			</image>
			<input placeholder="账号" maxlength="12" />
		</view>
		<view class="row-input">
			<image mode="aspectFit"
				src="./static/login9_password.png">
			</image>
			<input placeholder="请输入密码" password maxlength="18" />
		</view>
		<view class="login-btn">
			登录
		</view>
		<text class="menu-link" @click="gotoRegister">注册</text>
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
					url: '/pages/login9/register'
				})
			}
		}
	}
</script>

<style lang="scss">
	.content {
		padding: 60rpx;
	}

	.tip-box {
		padding-bottom: 30rpx;
		display: flex;
		flex-direction: column;

		.tip {
			position: relative;
			height: 100rpx;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: flex-start;
			font-size: 60rpx;
			letter-spacing: 5rpx;
			z-index: 1;

			.thing {
				position: absolute;
				bottom: 2rpx;
				width: 130rpx;
				height: 20rpx;
				z-index: -1;
				background: linear-gradient(to right, #f7d675, #edb146);
			}
		}


		.sub-tip {
			line-height: 80rpx;
			color: #cccccc;
			font-size: 26rpx;
			letter-spacing: 10rpx;
		}
	}

	.row-input {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		height: 150rpx;
		border-bottom: 1rpx solid #f1f1f1;

		image {
			margin: 0 20rpx;
			width: 40rpx;
			height: 40rpx;
		}

		input {
			padding-left: 20rpx;
			font-size: 30rpx;
		}
	}

	.login-btn {
		margin-top: 70rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 50rpx;
		width: 100%;
		height: 100rpx;
		color: #FFFFFF;
		letter-spacing: 5rpx;
		font-size: 30rpx;
		background: linear-gradient(to right, #f7d675, #edb146);
	}

	.menu-link {
		letter-spacing: 5rpx;
		display: flex;
		justify-content: center;
		line-height: 120rpx;
		font-size: 26rpx;
	}
</style>
