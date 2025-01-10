<template>
	<view class="content">
		<view class="form-box">
			<view class="login-box">
				<text class="name">Sign in</text>
				<image class="logo" mode="aspectFill" src="./static/login57_logo.png">
				</image>
			</view>
			<view class="row-input">
				<input placeholder="Arian" maxlength="12" />
				<image src="./static/login57_account.png">
				</image>
			</view>
			<view class="row-input">
				<input placeholder="Password" maxlength="18" password />
				<image src="./static/login57_password.png">
				</image>
			</view>
			<view class="login-btn">SIGN IN</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				logo: "",
				account: "",
				password: "",
			}
		},
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #636363;
	}

	.form-box {
		position: fixed;
		padding: 100rpx 40rpx;
		top: 16vh;
		left: 40rpx;
		right: 40rpx;
		border-radius: 20rpx;
		background-color: #0b0c0e;

		.login-box {
			display: flex;
			justify-content: space-between;
			align-items: center;
			width: 100%;
			height: 200rpx;

			.name {
				line-height: 120rpx;
				font-size: 50rpx;
				letter-spacing: 5rpx;
				font-weight: bold;
				color: #808080;
			}

			.logo {
				width: 140rpx;
				height: 140rpx;
			}
		}


		.row-input {
			margin: 20rpx 0;
			padding: 0 40rpx;
			height: 110rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;
			background-color: #0f1014;

			image {
				width: 40rpx;
				height: 40rpx;
				flex-shrink: 0;
			}

			input {
				flex: 1;
				color: #636363;
			}
		}


		.login-btn {
			margin-top: 50rpx;
			height: 110rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			color: #6b3e18;
			font-size: 30rpx;
			font-weight: bold;
			background-color: #dfa746;
		}
	}
</style>