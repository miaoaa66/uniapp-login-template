<template>
	<view class="content">
		<view class="tip-box">
			<text class="tip">用户登录</text>
			<text class="sub-tip">Login</text>
		</view>
		<view class="row-input">
			<text class="name">用户</text>
			<view class="input-box">
				<image mode="aspectFit" src="@/static/logo.png">
				</image>
				<input placeholder="Username" maxlength="12" />
			</view>
		</view>
		<view class="row-input">
			<text class="name">密码</text>
			<view class="input-box">
				<image mode="aspectFit" src="@/static/logo.png">
				</image>
				<input placeholder="Password" password maxlength="18" />
			</view>
		</view>
		<view class="login-btn">
			登录
		</view>
		<text class="menu-link" @click="gotoRegister">用户注册</text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				account: "",
				password: "",
			}
		},
		methods: {
			// 去注册
			gotoRegister() {
				uni.redirectTo({
					url: '/pages/login58/register'
				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		background: radial-gradient(at top left, #cddbfc, #f4e9db)
	}


	.content {
		padding: 60rpx;
	}

	.tip-box {
		height: 100rpx;
		padding-bottom: 50rpx;
		display: flex;
		justify-content: flex-start;
		align-items: flex-end;

		.tip {
			font-size: 50rpx;
			font-weight: bold;
			color: #1c6cdd;
		}

		.sub-tip {
			padding-left: 20rpx;
			font-size: 36rpx;
			color: #848484;
		}
	}

	.row-input {
		display: flex;
		flex-direction: column;
		justify-content: flex-start;

		.name {
			line-height: 100rpx;
			font-size: 36rpx;
			color: #1c6cdd;
			font-weight: bold;
		}

		.input-box {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 100rpx;
			border-radius: 10rpx;
			background-color: #e6f2fe;

			image {
				margin: 0 20rpx;
				width: 40rpx;
				height: 40rpx;
			}

			input {
				flex: 1;
				font-size: 30rpx;
			}
		}
	}

	.login-btn {
		margin-top: 70rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 10rpx;
		width: 100%;
		height: 100rpx;
		color: #FFFFFF;
		letter-spacing: 10rpx;
		font-size: 30rpx;
		background: linear-gradient(to right, #3e7ee2, #1c6cdd);
	}

	.menu-link {
		letter-spacing: 6rpx;
		display: flex;
		justify-content: center;
		line-height: 120rpx;
		font-weight: bold;
		font-size: 30rpx;
	}
</style>