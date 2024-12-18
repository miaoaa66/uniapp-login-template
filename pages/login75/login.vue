<template>
	<view class="content">
		<view class="head">
			<image mode="aspectFill" src="./static/login_head.png"></image>
		</view>
		<view class="title">
			<text class="name">DEEP SLEEP</text>
			<text class="sub-name">let`s get rid of insomnia</text>
		</view>
		<view class="login-btn">
			Login
		</view>
		<view class="menu-link">Not a member?
			<text class="join-now" @click="goto">Join now</text>
		</view>
		<text class="quick-link">Or sign in with</text>
		<view class="quick-img">
			<image src="./static/fs.png"></image>
			<image src="./static/github.png"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		methods: {
			goto() {
				uni.navigateTo({
					url: "/pages/login75/register"
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #cfd6dc;
	}

	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		height: 100%;
		width: 100%;


		.head {
			width: 100%;
			height: 30vh;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.title {
			display: flex;
			align-items: center;
			flex-direction: column;
			width: 100%;
			height: 20vh;

			.name {
				display: flex;
				align-items: flex-end;
				height: 180rpx;
				font-size: 80rpx;
				font-weight: bold;
				color: #000208;
			}

			.sub-name {
				line-height: 100rpx;
				font-size: 32rpx;
				color: #90969b;
			}
		}

		.login-btn {
			margin: 80rpx 0;
			display: flex;
			justify-content: center;
			align-items: center;
			width: 600rpx;
			height: 110rpx;
			color: #a3a3a3;
			border-radius: 6rpx;
			font-size: 40rpx;
			background-color: #000000;
		}

		.menu-link {
			font-size: 30rpx;
			color: #90969b;

			.join-now {
				padding-left: 10rpx;
				color: #000000;
			}
		}

		.quick-link {
			display: flex;
			align-items: flex-end;
			height: 200rpx;
			font-size: 30rpx;
			color: #90969b;
		}

		.quick-img {
			padding-top: 40rpx;
			display: flex;
			justify-content: center;

			image {
				width: 100rpx;
				height: 100rpx;
				border-radius: 50rpx;
				margin: 0 20rpx;
			}
		}

	}
</style>