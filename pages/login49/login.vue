<template>
	<view class="form-box">
		<view class="row-box">
			<view class="row">
				<input class="input-box" placeholder-class="row-placeholder" type="text" placeholder="Your Name" />
			</view>
			<view class="img-box">
				<view class="upper">
					<image mode="aspectFill" src="./static/login49_account.png"></image>
				</view>
				<view class="below"></view>
			</view>
		</view>
		<view class="row-box">
			<view class="row">
				<input class="input-box" placeholder-class="row-placeholder" type="text" placeholder="Your Email" />
			</view>
			<view class="img-box">
				<view class="upper">
					<image mode="aspectFill" src="./static/login49_email.png"></image>
				</view>
				<view class="below"></view>
			</view>
		</view>
		<view class="row-box">
			<view class="row">
				<input class="input-box" placeholder-class="row-placeholder" password="true" type="text"
					placeholder="Your Password" />
			</view>
			<view class="img-box">
				<view class="upper">
					<image mode="aspectFill" src="./static/login49_password.png"></image>
				</view>
				<view class="below"></view>
			</view>
		</view>
		<view class="login-box">
			<view class="login">LOGIN</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		methods: {}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #f7f0da;
	}

	.form-box {
		margin-top: 20vh;
		display: flex;
		flex-direction: column;
		align-items: center;

		.row-box {
			position: relative;
			display: flex;
			justify-content: center;
			align-items: flex-start;
			margin: 20rpx 0;
			width: 680rpx;
			height: 120rpx;
			background-color: #c0c0c0;
			border: 1rpx solid #9d9d9d;
			border-radius: 10rpx;
			box-shadow: rgba(12, 19, 25, 0.4) 0rpx 6rpx 12rpx 0rpx;

			.row {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 99%;
				height: 100rpx;
				background-color: #ffffff;
				border-radius: 6rpx;

				.input-box {
					padding: 0 200rpx 0 20rpx;
					width: 420rpx;
					height: 60rpx;
					border: 1rpx solid #b9b9b9;
					border-radius: 4rpx;
				}

				.row-placeholder {
					color: #d94a2d;
				}
			}

			.img-box {
				position: absolute;
				top: 0;
				right: 80rpx;
				width: 120rpx;
				display: flex;
				flex-direction: column;

				.upper {
					display: flex;
					justify-content: center;
					align-items: center;
					width: 100%;
					height: 100rpx;
					background-color: #e37f6a;

					image {
						width: 50rpx;
						height: 50rpx;
					}
				}

				.below {
					width: 100%;
					height: 20rpx;
					background-color: #9c2912;
				}
			}
		}

		.login-box {
			display: flex;
			justify-content: center;
			align-items: flex-start;
			margin: 60rpx;
			width: 280rpx;
			height: 100rpx;
			background-color: #5f8214;
			border-top-left-radius: 10rpx;
			border-top-right-radius: 10rpx;
			border-bottom-left-radius: 16rpx;
			border-bottom-right-radius: 16rpx;
			border: 4rpx solid #47640a;

			.login {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 100%;
				height: 80rpx;
				font-size: 50rpx;
				font-weight: bold;
				letter-spacing: 2rpx;
				color: #4c6e03;
				background-color: #b0cf6a;
				border-radius: 10rpx;
			}
		}
	}
</style>