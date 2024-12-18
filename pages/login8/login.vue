<template>
	<view class="content">
		<view class="head">
			<image class="img1" v-show="checked" mode="aspectFit"
				src="./static/login8_bg1.png">
			</image>
			<image class="img2" v-show="!checked" mode="aspectFit"
				src="./static/login8_bg2.png">
			</image>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image mode="aspectFit"
					src="./static/login8_account.png">
				</image>
				<input placeholder="用户名/邮箱/手机号" maxlength="18" />
			</view>
			<view class="row-input">
				<image mode="aspectFit"
					src="./static/login8_password.png">
				</image>
				<input placeholder="密码" maxlength="18" password />
			</view>
			<view class="menu-link">
				<view class="half">
					<checkbox :checked="checked" @click="checkClick" style="transform: scale(0.6);" />
					<text>记住密码</text>
				</view>
				<view class="half forget">
					<text>忘记密码</text>
				</view>
			</view>
			<view class="login-btn">
				登录
			</view>
			<text class="register-link">注册新账号</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				checked: false
			}
		},
		methods: {
			checkClick(e) {
				this.checked = !this.checked
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #ffaf40;
	}

	.head {
		position: relative;
		height: 30vh;
		background-color: #ff9000;

		image {
			width: 550rpx;
			height: 310rpx;
		}

		.img1 {
			position: absolute;
			left: 100rpx;
			bottom: 0;
		}

		.img2 {
			position: absolute;
			left: 100rpx;
			bottom: -150rpx;
		}
	}

	.form-box {
		padding: 100rpx 80rpx;
		height: 100%;

		.row-input {
			margin-top: 50rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;
			color: #d1ccc3;
			height: 100rpx;
			font-size: 30rpx;
			border-radius: 50rpx;
			background-color: #ffffff;

			image {
				padding: 0rpx 40rpx;
				width: 70rpx;
				height: 70rpx;
			}
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
				color: #fada86;
			}

			.forget {
				display: flex;
				justify-content: flex-end;
			}
		}

		.login-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			color: #f1ebbc;
			width: 600rpx;
			height: 100rpx;
			font-size: 32rpx;
			border-radius: 50rpx;
			background: linear-gradient(to right, #a76102, #be812e);
		}

		.register-link {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 100rpx;
			font-size: 30rpx;
			color: #f1ebbc;
			text-decoration: underline;
		}
	}
</style>
