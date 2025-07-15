<template>
	<view class="content">
		<view class="title">欢迎使用XXXX系统</view>
		<image class="logo" src="./static/login88_logo.png"></image>
		<view class="form-box">
			<view class="row">
				<view class="name">手机号</view>
				<view class="input-box">
					<input placeholder="请输入手机号" maxlength="11" />
				</view>
			</view>
			<view class="row">
				<view class="name">验证码</view>
				<view class="input-box">
					<input placeholder="请输入验证码" maxlength="6" />
					<text>获取验证码</text>
				</view>
			</view>
			<view class="login-btn">
				立即登录
			</view>
		</view>
		<view class="tip-link">
			查看<text>演示效果</text>及<text>武林秘籍</text>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
			}
		},
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #f1f1f1;
	}

	.title {
		line-height: 300rpx;
		padding-left: 50rpx;
		font-size: 40rpx;
		font-weight: bold;
	}

	.logo {
		position: absolute;
		top: 280rpx;
		left: 80rpx;
		width: 120rpx;
		height: 120rpx;
		z-index: 999;
	}


	.form-box {
		position: relative;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: flex-start;
		margin: 0 40rpx;
		padding: 200rpx 20rpx 80rpx 20rpx;
		border-radius: 20rpx;
		background-color: #ffffff;
		clip-path: polygon(0% 15%,
				100% 0%,
				100% 100%,
				0% 100%);

		.row {
			display: flex;
			flex-direction: column;
			width: 85%;
			height: 200rpx;
			font-size: 30rpx;

			.name {
				height: 60rpx;
				font-weight: bold;
			}

			.input-box {
				display: flex;
				justify-content: flex-start;
				padding: 0 20rpx;
				align-items: center;
				height: 90rpx;
				border-radius: 100rpx;
				background-color: #f5f7f6;

				input {
					flex: 1;
				}

				text {
					font-size: 26rpx;
					width: 140rpx;
				}
			}
		}

		.login-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			margin-top: 20rpx;
			width: 85%;
			height: 90rpx;
			border-radius: 100rpx;
			font-size: 30rpx;
			letter-spacing: 4rpx;
			color: #f5f7f6;
			background-color: #3e3f41;
		}
	}

	.tip-link {
		display: flex;
		justify-content: center;
		letter-spacing: 4rpx;
		line-height: 150rpx;
		font-size: 26rpx;
		color: #808286;

		text {
			color: #3e3f41;
			font-weight: bold;
		}
	}
</style>