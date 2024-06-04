<template>
	<view class="content">
		<view class="row-input">
			<text class="name">用户账号</text>
			<input placeholder="手机号码或邮箱" maxlength="18" />
		</view>
		<view class="row-input">
			<text class="name">验证码</text>
			<input placeholder="输入验证码" maxlength="6" />
			<view class="code">
				<text>获取验证码</text>
			</view>
		</view>
		<view class="row-input">
			<text class="name">登录密码</text>
			<input placeholder="6~16位字母和数字结合" maxlength="16" />
		</view>
		<view class="row-input">
			<text class="name">确认密码</text>
			<input placeholder="再次输入您的密码" maxlength="16" />
		</view>
		<view class="forget-btn">提交</view>
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
	page {
		background-color: #0b0c14;
	}

	.content {
		padding: 40rpx;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 110rpx;
			padding: 0 30rpx;
			background-color: #2b2f41;
			border-radius: 8rpx;
			margin-bottom: 50rpx;
			font-size: 30rpx;

			.name {
				width: 150rpx;
				letter-spacing: 4rpx;
				color: #FFFFFF;
			}

			input {
				flex: 1;
				color: #a1a2a3;
			}

			.code {
				display: flex;
				justify-content: flex-end;
				letter-spacing: 2rpx;
				width: 180rpx;
				color: #1e70fe;
			}
		}

		.forget-btn {
			margin-top: 60rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 35rpx;
			letter-spacing: 15rpx;
			color: #FFFFFF;
			height: 110rpx;
			border-radius: 8rpx;
			background-image: linear-gradient(to right, #3feaea, #0168e6);
		}
	}
</style>
