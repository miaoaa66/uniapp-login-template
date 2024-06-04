<template>
	<view class="content">
		<view class="head">
			<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login7/login7_logo.png"></image>
		</view>
		<view class="menu-btn" @click="gotoLogin">登录</view>
		<view class="menu-btn" @click="gotoRegister">注册</view>
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
					url: '/pages/login7/login'
				})
			},

			// 去注册
			gotoRegister() {
				uni.navigateTo({
					url: '/pages/login7/register'
				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #ce4b3b;
	}

	.content {
		margin: 0rpx 100rpx;

		.head {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 45vh;

			image {
				width: 360rpx;
				height: 198rpx;
			}
		}

		.menu-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			color: #ce4b3b;
			margin: 50rpx 0rpx;
			height: 100rpx;
			font-weight: bold;
			font-size: 35rpx;
			letter-spacing: 5rpx;
			background-color: #f1d4cb;
			border-radius: 35rpx;
		}

	}
</style>
