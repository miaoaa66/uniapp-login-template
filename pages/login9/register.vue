<template>
	<view class="content">
		<view class="tip-box">
			<view class="tip">
				<text>注册</text>
				<text class="thing"></text>
			</view>
			<text class="sub-tip">已有账号，马上登录</text>
		</view>
		<view class="row-input">
			<input placeholder="手机" maxlength="11" />
		</view>
		<view class="row-input">
			<input placeholder="验证码" maxlength="6" />
			<view class="code">
				<text>请输入验证码</text>
			</view>
		</view>
		<view class="row-input">
			<input placeholder="密码" password maxlength="18" />
		</view>
		<view class="row-input">
			<input placeholder="确认密码" password maxlength="18" />
		</view>
		<view class="login-btn">
			注册
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
	.content {
		padding: 60rpx;
	}

	.tip-box {
		padding-bottom: 30rpx;
		display: flex;
		flex-direction: column;

		.tip {
			position: relative;
			height: 100rpx;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: flex-start;
			font-size: 60rpx;
			letter-spacing: 5rpx;
			z-index: 1;

			.thing {
				position: absolute;
				bottom: 2rpx;
				width: 130rpx;
				height: 20rpx;
				z-index: -1;
				background: linear-gradient(to right, #f7d675, #edb146);
			}
		}


		.sub-tip {
			line-height: 80rpx;
			color: #cccccc;
			font-size: 26rpx;
			letter-spacing: 10rpx;
		}
	}

	.row-input {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		height: 150rpx;
		border-bottom: 1rpx solid #f1f1f1;

		input {
			padding-left: 20rpx;
			font-size: 30rpx;
		}

		.code {
			display: flex;
			justify-content: flex-end;
			width: 300rpx;
			font-size: 28rpx;
			color: #eeb44a;
		}
	}

	.login-btn {
		margin-top: 70rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 50rpx;
		width: 100%;
		height: 100rpx;
		color: #FFFFFF;
		letter-spacing: 5rpx;
		font-size: 30rpx;
		background: linear-gradient(to right, #f7d675, #edb146);
	}
</style>
