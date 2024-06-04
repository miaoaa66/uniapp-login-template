<template>
	<view class="content">
		<image class="bg" mode="aspectFit" src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login29/login29_bg.png"></image>
		<view class="box">
			<view class="login-btn" @click="gotoLogin">登录</view>
			<view class="tip-link">还没有账号?快来
				<text @click="gotoRegister">注册</text>
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
			// 去登录
			gotoLogin() {
				uni.navigateTo({
					url: '/pages/login29/login'
				})
			},
			// 去注册
			gotoRegister() {
				uni.navigateTo({
					url: '/pages/login29/register'
				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #e0edfe;
	}

	.content {
		position: relative;
	}

	.bg {
		position: fixed;
		top: 15vh;
		left: 100rpx;
		width: 600rpx;
		height: 700rpx;
		z-index: -1;
	}

	.box {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		position: fixed;
		left: 40rpx;
		right: 40rpx;
		bottom: 80rpx;
	}

	.login-btn {
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 40rpx;
		letter-spacing: 10rpx;
		font-weight: bold;
		color: #FFFFFF;
		background-image: linear-gradient(to right, #a1c4fc, #998cf5);
		box-shadow: 0rpx 5rpx 30rpx #cabdf5;
		height: 110rpx;
		border-radius: 50rpx;
		width: 100%;
	}

	.tip-link {
		line-height: 120rpx;
		letter-spacing: 5rpx;
		font-size: 26rpx;

		text {
			padding-left: 10rpx;
			color: #007AFF;
		}

	}
</style>
