<template>
	<view class="content">
		<view class="title">Sign Up</view>
		<view class="form-box">
			<view class="row-input">
				<input placeholder="Full Name" maxlength="11" />
				<image mode="aspectFill"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login53/login53_account.png">
				</image>
			</view>
			<view class="row-input">
				<input placeholder="Email Address" maxlength="16" />
				<image mode="aspectFill"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login53/login53_email.png">
				</image>
			</view>
			<view class="row-input">
				<input placeholder="Password" password maxlength="18" />
				<image mode="aspectFill"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login53/login53_password.png">
				</image>
			</view>
			<view class="login-btn">Create Account</view>
			<view class="menu-link">
				<text @click="login">Login Account?</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		methods: {
			login() {
				uni.redirectTo({
					url: "/pages/login53/login"
				})
			}
		}
	}
</script>

<style lang="scss">
	.title {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 200rpx;
		font-size: 70rpx;
		letter-spacing: 4rpx;
		font-weight: bold;
	}

	.form-box {
		padding: 40rpx 60rpx;

		.row-input {
			display: flex;
			justify-content: flex-end;
			align-items: center;
			margin: 40rpx 0;
			height: 100rpx;
			font-size: 30rpx;
			border-radius: 50rpx;
			border: 4rpx solid #bcbcbc;

			image {
				margin: 0 30rpx;
				flex-shrink: 0;
				width: 30rpx;
				height: 30rpx;
			}

			input {
				padding-left: 30rpx;
				font-size: 30rpx;
				flex: 1;
			}
		}

		.menu-link {
			display: flex;
			flex-direction: column;
			align-items: center;
			line-height: 100rpx;
			font-size: 28rpx;
			letter-spacing: 3rpx;
			color: #fdb216;
		}

		.login-btn {
			margin: 40rpx 0;
			display: flex;
			justify-content: center;
			align-items: center;
			border-radius: 50rpx;
			width: 100%;
			height: 100rpx;
			font-size: 40rpx;
			color: #FFFFFF;
			background-color: #fb6e50;
			box-shadow: 0rpx 2rpx 20rpx #fb6e50;
		}
	}
</style>