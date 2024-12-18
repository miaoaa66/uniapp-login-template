<template>
	<view class="content">
		<view class="couple">
			<view class="left-rope rope"></view>
			<view class="right-rope rope"></view>
		</view>
		<view class="form-box">
			<view class="head-box">
				<text class="name">登录界面</text>
				<view class="close">x</view>
			</view>
			<view class="within">
				<view class="input-box">
					<text>账号：</text>
					<input :value="account" maxlength="11" />
				</view>
				<view class="input-box">
					<text>密码：</text>
					<input :value="password" maxlength="11" password="true" />
				</view>
				<view class="btn-box">
					<view class="login">登录</view>
					<view class="resetting" @click="resettingClick">重置</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				account: "张三",
				password: "******"
			}
		},
		methods: {
			// 重置
			resettingClick() {
				this.account = ""
				this.password = ""
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #414246;
	}

	.content {
		padding-top: 20vh;
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.couple {
		position: relative;
		width: 20rpx;
		height: 20rpx;
		border-radius: 100%;
		border: 10rpx solid #c5ccd0;
		background-color: #e5e5e5;
		z-index: 4;

		.rope {
			position: absolute;
			width: 300rpx;
			height: 4rpx;
			background-color: #e5e5e5;
			z-index: 2;
		}

		.left-rope {
			top: 120rpx;
			left: -260rpx;
			transform: rotate(135deg);
		}

		.right-rope {
			top: 120rpx;
			right: -260rpx;
			transform: rotate(45deg);
		}
	}

	.form-box {
		display: flex;
		flex-direction: column;
		justify-content: flex-end;
		align-items: center;
		margin: 190rpx auto 0 auto;
		border-radius: 10rpx;
		width: 650rpx;
		height: 500rpx;
		background-image: radial-gradient(#d4dbe5, #c6d2e1, #dfe7ec);
		border: 2rpx solid #282b2c;
		z-index: 6;

		.head-box {
			position: relative;
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 100rpx;

			.name {
				font-size: 30rpx;
			}

			.close {
				position: absolute;
				display: flex;
				justify-content: center;
				top: 0;
				right: 10rpx;
				width: 100rpx;
				font-size: 35rpx;
				border-bottom-left-radius: 10rpx;
				border-bottom-right-radius: 10rpx;
				border: 2rpx solid #555555;
				color: #ffffff;
				background-image: linear-gradient(#ecbab2, #b95c52, #cb5e49);
			}
		}

		.within {
			width: 96%;
			height: 80%;
			margin-bottom: 10rpx;
			background-color: #efefef;
			border: 2rpx solid #282b2c;
			border-radius: 2rpx;

			.input-box {
				display: flex;
				justify-content: center;
				align-items: center;
				margin: 40rpx 0;
				border-radius: 10rpx;
				border-radius: 2rpx;
				font-size: 28rpx;

				text {
					width: 20%;
				}

				input {
					padding: 0 20rpx;
					height: 65rpx;
					border: 2rpx solid #b0b5ba;
					background-color: #ffffff;
				}
			}

			.btn-box {
				display: flex;
				justify-content: space-around;
				align-items: center;

				.login,
				.resetting {
					display: flex;
					justify-content: center;
					align-items: center;
					width: 200rpx;
					height: 60rpx;
					font-size: 28rpx;
					border-radius: 2rpx;
					border: 2rpx solid #969fa7;
					background-image: linear-gradient(#e9f0f9, #f2f8fc, #c7daea);
				}
			}

		}


	}
</style>