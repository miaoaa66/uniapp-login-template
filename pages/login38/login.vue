<template>
	<view class="content">
		<image class="bg" mode="aspectFill"
			src="./static/login38_bg.png"></image>
		<view class="form-box">
			<view class="title-box">
				<view class="title">开启你的精彩世界</view>
				<view class="title-sub">首次登录将自动创建账号</view>
			</view>
			<view class="btn-box">
				<view class="btn onekey">手机一键登录</view>
				<view class="btn authorize">微信授权登录</view>
				<view class="agreement">查看
					<text class="link">演示效果</text>和<text class="link">演示效果</text>
				</view>
			</view>

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
	}
</script>

<style lang="scss">
	.bg {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: -1;
		width: 100%;
		height: 100%;
	}

	.form-box {
		margin: 16vh 40rpx 0rpx 40rpx;
		height: 65vh;
		display: flex;
		flex-direction: column;
		justify-content: space-between;

		.title-box {
			display: flex;
			flex-direction: column;

			.title {
				height: 80rpx;
				letter-spacing: 2rpx;
				font-size: 45rpx;
				font-weight: bold;
			}

			.title-sub {
				font-size: 24rpx;
				letter-spacing: 2rpx;
			}
		}


		.btn-box {

			.btn {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 100%;
				height: 90rpx;
				border-radius: 10rpx;
				font-size: 30rpx;
				letter-spacing: 2rpx;
				margin: 30rpx 0rpx;
			}

			.onekey {
				color: #FFFFFF;
				background-color: #fd3948;
			}

			.authorize {
				color: #000;
				background-color: #ffffff;
			}

			.agreement {
				display: flex;
				justify-content: center;
				letter-spacing: 5rpx;
				font-size: 24rpx;

				.link {
					color: #007AFF;
					text-decoration: underline;
				}
			}
		}

	}
</style>