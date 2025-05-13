<template>
	<view class="content">
		<view class="title">
			<text class="name">Wellcome</text>
			<text class="sub-name">Automatic identity verification</text>
			<text class="sub-name">which Enable you to verify</text>
		</view>
		<view class="login">
			<image mode="aspectFill" src="./static/login83_login.png"></image>
		</view>
		<view class="btn" @click="register">
			Log in
		</view>
		<view class="btn sign-up" @click="register">
			Sign up
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		onLoad() {
		},
		methods: {
			register() {
				uni.navigateTo({
					url: "/pages/login83/register"
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #fef4e8;
	}

	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		height: 100%;
		width: 100%;


		.login {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 30vh;

			image {
				width: 500rpx;
				height: 500rpx;
			}
		}

		.title {
			display: flex;
			justify-content: center;
			align-items: center;
			flex-direction: column;
			width: 100%;
			height: 30vh;

			.name {
				display: flex;
				align-items: center;
				height: 200rpx;
				font-size: 80rpx;
				font-weight: bold;
				color: #000208;
			}

			.sub-name {
				line-height: 60rpx;
				font-size: 30rpx;
				color: #90969b;
			}
		}

		.btn {
			margin: 40rpx 0 0 0;
			display: flex;
			justify-content: center;
			align-items: center;
			font-weight: bold;
			width: 600rpx;
			height: 110rpx;
			color: #000000;
			font-size: 40rpx;
			border: 4rpx solid #000000;
			border-radius: 100rpx;
		}

		.sign-up {
			background-color: #fdde64;
		}
	}
</style>