<template>
	<view class="content">
		<image class="bg" src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login23/login23_bg.png"></image>
		<view class="logo">
			LOGO
		</view>
		<view class="form-box">
			<view class="row-input">
				<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login23/login23_account.png"></image>
				<input placeholder="用户名" maxlength="12" />
			</view>
			<view class="row-input">
				<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login23/login23_password.png"></image>
				<input placeholder="密码" password maxlength="18" />
			</view>
			<view class="login-btn">登录</view>
			<view class="menu-link">
				<text>立即注册</text>
				<text>忘记密码?</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #ffdcc8;
	}

	.bg {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		z-index: -1;
	}

	.logo {
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 60rpx;
		color: #FFFFFF;
		font-weight: bold;
		letter-spacing: 5rpx;
		height: 30vh;
	}

	.form-box {
		position: fixed;
		top: 27vh;
		left: 110rpx;
		right: 110rpx;
		border-radius: 40rpx;
		border: 5rpx solid #c28016;
		background-color: #FFFFFF;
		padding: 80rpx 40rpx;

		.row-input {
			padding-top: 20rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 100rpx;
			border-bottom: 1rpx solid #c6c6c6;

			image {
				margin-right: 20rpx;
				width: 40rpx;
				height: 40rpx;
				flex-shrink: 0;
			}

			input {
				flex: 1;
			}
		}

		.login-btn {
			margin-top: 50rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 34rpx;
			width: 100%;
			height: 80rpx;
			border-radius: 50rpx;
			color: #FFFFFF;
			letter-spacing: 5rpx;
			border: 5rpx solid #c28016;
			background-color: #a2d6fe;
		}

		.menu-link {
			display: flex;
			justify-content: space-between;
			align-items: center;
			font-size: 26rpx;
			color: #fbc4cb;
			line-height: 80rpx;
			letter-spacing: 5rpx;
		}
	}
</style>
