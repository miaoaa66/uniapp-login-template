<template>
	<view class="content">
		<view class="head">
			<image class="logo" mode="aspectFill" src="@/static/logo.png">
			</image>
			<text class="name">RELATE</text>
		</view>
		<view class="form-box">
			<view class="row-input" :class="selectIndex==0?'row-input-selected':''">
				<image mode="aspectFit" src="@/static/logo.png">
				</image>
				<input placeholder="Username" placeholder-class="input-placeholder" maxlength="18"
					@focus="focusUsername" />
			</view>
			<view class="row-input" :class="selectIndex==1?'row-input-selected':''">
				<image mode="aspectFit" src="@/static/logo.png">
				</image>
				<input placeholder="Passwoord" placeholder-class="input-placeholder" maxlength="18" password
					@focus="focusPassword" />
			</view>
			<view class="login-btn">
				GET STARTED
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				selectIndex: 0,
				logo: "",
				account: "",
				password: "",
			}
		},
		methods: {
			focusUsername() {
				this.selectIndex = 0;
			},

			focusPassword() {
				this.selectIndex = 1;
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-image: linear-gradient(#ffffff, #000000);
		min-height: 100vh;
	}

	.head {
		height: 30vh;
		display: flex;
		justify-content: flex-end;
		align-items: center;
		flex-direction: column;

		.logo {
			width: 180rpx;
			height: 180rpx;
		}

		.name {
			padding-top: 40rpx;
			font-size: 40rpx;
			letter-spacing: 10rpx;
			font-weight: bold;
		}
	}

	.form-box {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-top: 60rpx;
		height: 100%;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			color: #d1ccc3;
			height: 140rpx;
			width: 540rpx;
			font-size: 30rpx;
			background-color: #fff;
			box-shadow: 0rpx 5rpx 20rpx #dad9d9;

			image {
				padding: 0rpx 40rpx;
				width: 40rpx;
				height: 40rpx;
			}

			.input-placeholder {
				font-size: 28rpx;
				color: #9b9b9b;
			}
		}

		.row-input-selected {
			width: 620rpx;
			height: 160rpx;
		}

		.login-btn {
			margin-top: 40rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			width: 540rpx;
			height: 120rpx;
			font-weight: bold;
			font-size: 32rpx;
			box-shadow: 0rpx 5rpx 20rpx #eff3f3;
			background-image: linear-gradient(to right, #d4fed2, #acf9f8);
		}
	}
</style>