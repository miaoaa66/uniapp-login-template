<template>
	<view class="content">
		<view class="login-tip">
			<view class="title">Sign In</view>
			<view class="sub-title">Welcome Back</view>
		</view>
		<view class="form-box">
			<input placeholder-class="input-placeholder" placeholder="Email" maxlength="20" />
			<input placeholder-class="input-placeholder" placeholder="Password" maxlength="20" />
			<view class="radio-box">
				<radio checked="true" color="#4a6ef5" style="transform: scale(0.8)"></radio>
				<text>Keep me Logged in</text>
			</view>
			<view class="btn-box">Login </view>
			<text class="forget-password">Forgot Password</text>
		</view>
		<text class="sign-up">Sign Up</text>
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
	.content {
		padding: 180rpx 80rpx 100rpx 80rpx;

		.title {
			font-size: 60rpx;
			line-height: 100rpx;
		}

		.sub-title {
			line-height: 80rpx;
			font-size: 24rpx;
			color: #a1a1a2;
		}

		.form-box {
			margin-top: 40rpx;
			flex-direction: column;

			input {
				padding: 0 20rpx;
				margin: 30rpx 0;
				background-color: #f2f3f5;
				border-radius: 40rpx;
				height: 100rpx;
				font-size: 26rpx;
			}

			.radio-box {
				display: flex;
				height: 50rpx;
				font-size: 26rpx;

				text {
					height: 100%;
					padding-top: 10rpx;
					padding-left: 10rpx;
				}
			}

			.btn-box {
				margin-top: 50rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				height: 100rpx;
				background-color: #496df4;
				border-radius: 40rpx;
				color: #FFFFFF;
				font-size: 30rpx;
				box-shadow: 0rpx 20rpx 40rpx #859df4;
			}

			.forget-password {
				line-height: 120rpx;
				font-size: 28rpx;
				display: block;
				text-align: center;
			}

			.input-placeholder {
				color: #bcbcbc;
			}
		}
	}

	.sign-up {
		position: fixed;
		left: 325rpx;
		width: 100rpx;
		bottom: 100rpx;
		font-size: 26rpx;
		bottom: 180rpx;
		color: #6e87e5;
	}
</style>