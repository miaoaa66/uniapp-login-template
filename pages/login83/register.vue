<template>
	<view class="content">
		<view class="title">
			<text class="name">Sign up</text>
		</view>
		<view class="input-box">
			<input type="text" placeholder="Email" placeholder-class="input-placeholder" maxlength="16" />
		</view>
		<view class="input-box">
			<input type="text" placeholder="Password" placeholder-class="input-placeholder" maxlength="20" />
		</view>
		<view class="input-box">
			<input type="text" placeholder="Confirm Password" placeholder-class="input-placeholder" maxlength="16" />
		</view>
		<view class="login-btn">
			Sign up
		</view>
		<view class="menu-link">Already have an account?
			<text class="join-now" @click="goto">Log in</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		onLoad() {},
		methods: {
			goto() {
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #fffcf7;
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
			justify-content: flex-start;
			align-items: center;
			margin: 10rpx 0;
			width: 600rpx;
			height: 140rpx;
			font-size: 30rpx;
			border-bottom: 4rpx solid #757c82;

			input {
				text-align: center;
				flex: 1;
			}

			.input-placeholder {
				display: flex;
				justify-content: flex-start;
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
			color: #000000;
			font-size: 40rpx;
			background-color: #3ef7ce;
			border-radius: 100rpx;
			border: 4rpx solid #000000;
		}

		.menu-link {
			font-size: 30rpx;
			color: #90969b;

			.join-now {
				padding-left: 10rpx;
				color: #000000;
			}
		}

	}
</style>