<template>
	<view class="form-box">
		<view class="row-box">
			<view class="row">
				<input class="input-box" placeholder-class="row-placeholder" type="text" placeholder="Your Name" />
			</view>
		</view>
		<view class="row-box">
			<view class="row">
				<input class="input-box" placeholder-class="row-placeholder" type="text" placeholder="Your Email" />
			</view>
		</view>
		<view class="login-box">
			<view class="login">→</view>
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

<style lang="scss">
	page {
		background-color: #5e90d9;
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
			align-items: flex-end;
			margin: 20rpx 0;
			width: 680rpx;
			height: 120rpx;
			background-color: #163151;
			border: 1rpx solid #9d9d9d;
			border-radius: 10rpx;
			box-shadow: rgba(12, 19, 25, 0.4) 0rpx 6rpx 12rpx 0rpx;

			.row {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 99%;
				height: 110rpx;
				background-color: #ffffff;
				border-radius: 6rpx;

				.input-box {
					padding: 0 200rpx 0 20rpx;
					width: 420rpx;
					height: 70rpx;
					border: 4rpx solid #010011;
					background-color: #4adfb4;
					border-radius: 10rpx;
				}
			}
		}

		.login-box {
			display: flex;
			justify-content: center;
			align-items: flex-start;
			margin: 60rpx;
			width: 480rpx;
			height: 100rpx;
			background-color: #f7b842;
			border-radius: 50rpx;
			border: 4rpx solid #07111a;

			.login {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 100%;
				height: 85rpx;
				font-size: 100rpx;
				font-weight: bold;
				color: #f8b43d;
				background-color: #ffda65;
				border-radius: 50rpx;
				border-bottom: 4rpx solid #07111a;
			}
		}
	}
</style>