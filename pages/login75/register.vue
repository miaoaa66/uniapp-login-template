<template>
	<view class="content">
		<view class="head">
			<image mode="aspectFill" src="./static/register_head.png"></image>
		</view>
		<view class="title">
			<text class="name">CREATE YOUR ACCOUNT</text>
		</view>
		<view class="input-box">
			<input type="text" placeholder="Your name" placeholder-class="input-placeholder" maxlength="16"/>
		</view>
		<view class="input-box">
			<input type="text" placeholder="Email" placeholder-class="input-placeholder" maxlength="20"/>
		</view>
		<view class="input-box">
			<input type="text" placeholder="Password" placeholder-class="input-placeholder" maxlength="16"/>
		</view>
		<view class="login-btn">
			Register now
		</view>
		<view class="menu-link">Already have an account?
			<text class="join-now" @click="goto">Sign in</text>
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
					url: "/pages/login75/login"
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
			padding-top: 60rpx;
			width: 100%;
			height: 12vh;
			background-color: #000000;

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
			height: 15vh;

			.name {
				display: flex;
				align-items: flex-end;
				height: 180rpx;
				font-size: 50rpx;
				font-weight: bold;
				color: #000208;
			}
		}

		.input-box {
			display: flex;
			justify-content: center;
			align-items: center;
			margin: 10rpx 0;
			border-radius: 6rpx;
			width: 600rpx;
			height: 100rpx;
			font-size: 30rpx;
			border: 4rpx solid #757c82;
			
			input{
				text-align: center;
				flex: 1;
			}

			.input-placeholder {
				display: flex;
				justify-content: center;
				align-items: center;
				font-size: 30rpx;
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