<template>
	<view class="content">
		<view class="title">
			<text>用户登录</text>
		</view>
		<view class="bg-long"></view>
		<view class="bg-bottom"></view>
		<view class="bg-form">
			<view class="form-box">
				<view class="input-box">
					<image src="@/static/logo.png"></image>
					<input placeholder="请输入用户名" maxlength="12" />
				</view>
				<view class="input-box">
					<image src="@/static/logo.png"></image>
					<input password placeholder="请输入密码" maxlength="18" />
				</view>
			</view>
		</view>
		<view class="bg-login">
			<text>→</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				account: "",
				password: "",
			}
		},
		methods: {}
	}
</script>

<style lang="scss">
	page {
		background-color: #ebedf7;
	}

	.title {
		position: fixed;
		top: -200rpx;
		left: -80rpx;
		width: 400rpx;
		height: 400rpx;
		border-radius: 50rpx;
		background-color: #fbfbfd;
		transform: rotate(40deg);

		text {
			position: fixed;
			top: 240rpx;
			left: 180rpx;
			color: #6c5dc9;
			font-size: 40rpx;
			font-weight: bold;
			transform: rotate(-40deg);
		}
	}

	.bg-long {
		position: fixed;
		top: 480rpx;
		left: -220rpx;
		width: 650rpx;
		height: 350rpx;
		border-radius: 50rpx;
		background-color: #fbfbfd;
		transform: rotate(40deg);
	}

	.bg-bottom {
		position: fixed;
		bottom: -360rpx;
		left: -400rpx;
		width: 700rpx;
		height: 700rpx;
		border-radius: 50rpx;
		background-color: #6c5dc9;
		transform: rotate(40deg);
	}

	.bg-form {
		position: fixed;
		top: 100rpx;
		right: -400rpx;
		width: 900rpx;
		height: 900rpx;
		border-radius: 100rpx;
		background-color: #fbfbfd;
		transform: rotate(40deg);
		box-shadow: 0rpx 10rpx 20rpx #f0f0fc;

		.form-box {
			position: fixed;
			top: 500rpx;
			left: 70rpx;
			width: 420rpx;
			transform: rotate(-40deg);

			.input-box {
				display: flex;
				justify-content: flex-start;
				align-items: center;
				height: 120rpx;
				border-bottom: 1rpx solid #e3e0f3;

				image {
					width: 30rpx;
					height: 30rpx;
					padding: 0 20rpx;
				}

				input {
					font-size: 30rpx;
					flex: 1;
				}
			}
		}
	}

	.bg-login {
		position: fixed;
		bottom: 300rpx;
		right: 100rpx;
		width: 300rpx;
		height: 300rpx;
		border-radius: 50rpx;
		transform: rotate(40deg);
		background-color: #695cc5;

		text {
			position: fixed;
			top: 80rpx;
			left: 100rpx;
			font-size: 100rpx;
			color: #fff;
			font-weight: bold;
			transform: rotate(-40deg);
		}
	}
</style>