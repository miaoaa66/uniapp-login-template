<template>
	<view class="content">
		<image class="bg" src="./static/login20_bg.png"></image>
		<view class="title">WELCOME</view>
		<view class="form-box">
			<view class="row-input" style="width: 500rpx;">
				<input placeholder="请输入邮箱" maxlength="18" />
				<text class="tip">邮箱</text>
			</view>
			<view class="row-input" style="width: 500rpx;margin-left: 80rpx;">
				<input placeholder="请输入密码" password maxlength="18" />
				<text class="tip">密码</text>
			</view>
			<view class="menu-link">
				<text>忘记密码</text>
			</view>
			<view class="login-btn">
				<image src="./static/login20_arrow.png"></image>
			</view>
			<view class="link-tip">
				还没有账号?
				<text>去注册</text>
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

	.title {
		margin-left: 100rpx;
		padding-top: 60rpx;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		height: 30vh;
		font-size: 80rpx;
		font-weight: bold;
		-webkit-text-fill-color: #ffffff;
		-webkit-text-stroke: 3rpx #fe5c5c;
	}

	.form-box {
		display: flex;
		flex-direction: column;
		align-items: center;
		position: fixed;
		padding: 70rpx 0rpx 50rpx 0rpx;
		border-radius: 20rpx;
		top: 30vh;
		left: 60rpx;
		right: 60rpx;
		background-color: #FFFFFF;
		box-shadow: 0px 5px 20px #c5908a;

		.row-input {
			position: relative;
			display: flex;
			justify-content: center;
			align-items: center;
			margin: 50rpx 40rpx 0rpx 40rpx;
			height: 100rpx;
			border-radius: 12rpx;
			border: 1rpx solid #fedce4;

			.tip {
				display: flex;
				justify-content: center;
				align-items: center;
				position: absolute;
				width: 100rpx;
				color: #d88fa4;
				background-color: #FFFFFF;
				top: -16rpx;
				left: 50rpx;
				font-size: 24rpx;
			}
		}

		.menu-link {
			width: 100%;
			display: flex;
			justify-content: flex-end;
			line-height: 100rpx;
			padding-right: 100rpx;
			font-size: 26rpx;
			color: #d88fa4;
		}

		.login-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 140rpx;
			height: 140rpx;
			border-radius: 100%;
			background: linear-gradient(#feb07f, #ff757f);
			box-shadow: 0px 10px 10px #f9c6b5;

			image {
				width: 80rpx;
				height: 60rpx;
			}
		}

		.link-tip {
			font-size: 26rpx;
			line-height: 120rpx;
			color: #a5a5a5;
			letter-spacing: 3rpx;

			text {
				color: #ff7b7e;
			}
		}
	}
</style>
