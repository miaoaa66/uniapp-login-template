<template>
	<view class="content">
		<image class="bg"  mode="heightFix" src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login11/login11_bg.png"></image>
		<view class="title">
			Sign Up
		</view>
		<view class="row-input">
			<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login11/login11_account.png"></image>
			<input placeholder="Name" maxlength="12" placeholder-style="color: #FFFFFF;font-size:30rpx" />
		</view>
		<view class="row-input">
			<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login11/login11_email.png"></image>
			<input placeholder="Email" maxlength="18" password placeholder-style="color: #FFFFFF;font-size:30rpx;" />
		</view>
		<view class="row-input">
			<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login11/login11_password.png"></image>
			<input placeholder="Password" maxlength="18" password placeholder-style="color: #FFFFFF;font-size:30rpx;" />
		</view>
		<view class="row-input">
			<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login11/login11_code.png"></image>
			<input placeholder="Birthday" maxlength="8" password placeholder-style="color: #FFFFFF;font-size:30rpx;" />
		</view>
		<view class="bottom-box">
			<view class="register-btn">Join</view>
			<view class="link-tip">
				Don`t have an account?<text @click="gotoLogin">Sign In</text>
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
			// 去登录
			gotoLogin() {
				uni.navigateTo({
					url: '/pages/login11/login'
				})
			}
		}
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
		padding-top: 140rpx;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		height: 200rpx;
		padding-left: 60rpx;
		letter-spacing: 5rpx;
		font-size: 60rpx;
		color: #FFFFFF;
	}

	.row-input {
		padding: 50rpx 60rpx 0 60rpx;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		height: 100rpx;
		border-bottom: 1rpx solid #818181;

		image {
			width: 40rpx;
			height: 40rpx;
		}

		input {
			padding: 0 40rpx;
			color: #FFFFFF;
		}
	}


	.bottom-box {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;

		.register-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 150rpx;
			font-size: 38rpx;
			letter-spacing: 5rpx;
			color: #FFFFFF;
			background-color: #fc3166;
		}

		.link-tip {
			display: flex;
			justify-content: center;
			align-items: center;
			line-height: 120rpx;
			font-size: 30rpx;
			color: #c0bfbd;

			text {
				padding-left: 20rpx;
				color: #FFFFFF;
			}
		}
	}
</style>
