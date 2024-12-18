<template>
	<view class="content">
		<view class="head">
			<image src="./static/login7_logo.png"></image>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image src="./static/login7_phone.png"></image>
				<input maxlength="11" placeholder-class="placeholder" placeholder="输入电话"/>
			</view>
			<view class="row-input">
				<image src="./static/login7_password.png"></image>
				<input maxlength="18" password placeholder-class="placeholder" placeholder="输入密码"/>
			</view>
			<view class="row-input">
				<image src="./static/login7_password.png"></image>
				<input maxlength="18" password placeholder-class="placeholder" placeholder="再次输入密码"/>
			</view>
			<view class="select-box">
				<image src="./static/login7_tick.png"></image>
			</view>
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
		background-color: #ce4b3b;
	}

	.content {
		margin: 0rpx 80rpx;

		.head {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 25vh;

			image {
				width: 360rpx;
				height: 198rpx;
			}
		}

		.form-box {
			position: relative;
			padding: 20rpx 20rpx 80rpx 20rpx;
			background-color: #d9766a;
			border-radius: 50rpx;

			.row-input {
				display: flex;
				justify-content: flex-start;
				align-items: center;
				height: 100rpx;
				margin: 30rpx;
				border-radius: 35rpx;
				background-color: #f1d4cb;

				image {
					margin: 0rpx 20rpx;
					width: 50rpx;
					height: 50rpx;
					flex-shrink: 0;
				}

				.placeholder {
					font-size: 38rpx;
					letter-spacing: 5rpx;
					color: #e0785f;
				}

				input {
					padding: 0rpx 20rpx;
					color: #d94535;
					font-weight: bold;
					width: 260rpx;
				}

				.code {
					display: flex;
					justify-content: center;
					align-items: center;
					flex: 1;
					height: 100%;
					background-color: #f9eeeb;
					border-radius: 35rpx;
					font-size: 26rpx;
					font-weight: bold;
					color: #ce4b3b;
					letter-spacing: 5rpx;
				}
			}

			.select-box {
				position: absolute;
				left: 230rpx;
				width: 140rpx;
				height: 140rpx;
				border-radius: 50%;
				display: flex;
				justify-content: center;
				align-items: center;
				background-color: #ce4b3b;

				image {
					width: 120rpx;
					height: 120rpx;
				}
			}
		}

	}
</style>
