<template>
	<view class="content">
		<view class="head">
			<image class="logo" mode="aspectFill" src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login33/login33_logo.png"></image>
			<text class="name">登录系统</text>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image mode="aspectFit"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login33/login33_phone.png">
				</image>
				<input placeholder="请输入手机号" placeholder-class="input-placeholder" maxlength="18" />
			</view>
			<view class="row-input">
				<image mode="aspectFit"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login33/login33_password.png">
				</image>
				<input placeholder="请输入密码" placeholder-class="input-placeholder" maxlength="18" password />
			</view>
			<view class="login-btn">
				登录
			</view>
			<view class="menu-link">
				<view class="half">
					<text>忘记密码</text>
				</view>
				<view class="half forget">
					<text>注册</text>
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
		methods: {}
	}
</script>

<style lang="scss">
	.head {
		position: relative;
		height: 30vh;
		background-color: #3fad28;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;

		.logo {
			width: 180rpx;
			height: 180rpx;
		}
		
		.name{
			padding-top: 40rpx;
			font-size: 40rpx;
			letter-spacing: 2rpx;
			color: #fff;
			font-weight: bold;
		}
	}

	.form-box {
		padding: 60rpx 80rpx;
		height: 100%;

		.row-input {
			margin-top: 50rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;
			color: #d1ccc3;
			height: 100rpx;
			font-size: 30rpx;
			border-bottom: 1rpx solid #bababa;

			image {
				padding: 0rpx 40rpx;
				width: 40rpx;
				height: 40rpx;
			}

			.input-placeholder {
				font-size: 28rpx;
				color: #adadad;
			}
		}

		.login-btn {
			margin-top: 80rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			color: #fff;
			width: 600rpx;
			height: 100rpx;
			font-size: 32rpx;
			border-radius: 50rpx;
			background-color: #3fad28;
		}

		.menu-link {
			height: 100rpx;
			padding: 0 50rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;

			.half {
				display: flex;
				align-items: center;
				width: 50%;
				font-size: 28rpx;
				color: #adadad;
			}

			.forget {
				display: flex;
				justify-content: flex-end;
			}
		}
	}
</style>