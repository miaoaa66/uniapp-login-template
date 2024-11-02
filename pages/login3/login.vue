<template>
	<view class="content">
		<view class="head">
			<text>Hi,</text>
			<text>Welcome</text>
		</view>
		<view class="form-box">
			<view class="row-input">
				<text class="tip">{{phone!=''?'Phone number':''}}</text>
				<view class="input-box">
					<input v-model="phone" maxlength="11" placeholder="Phone number" placeholder-class="placeholder" />
					<image mode="aspectFit" v-show="phone==''?false:true" @click="close" src="./static/login3_close.png">
					</image>
				</view>
			</view>
			<view class="row-input">
				<text class="tip">{{password!=''?'Password':''}}</text>
				<view class="input-box">
					<input v-model="password" maxlength="18" password placeholder="password"
						placeholder-class="placeholder" />
				</view>
			</view>
			<view class="login-btn">
				Sing in
			</view>
		</view>
		<view class="menu-link">Don`t have an account?
			<text @click="signUp">Sign up</text>、<text @click="forget">Forget password</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				phone: 12345678910,
				password: ""
			}
		},
		methods: {
			// 注册
			signUp() {
				uni.navigateTo({
					url: '/pages/login3/register'
				})
			},

			// 忘记密码
			forget() {
				uni.navigateTo({
					url: '/pages/login3/forget'
				})
			},

			// 关闭
			close() {
				this.phone = ""
			}
		}
	}
</script>

<style lang="scss">
	.content {
		padding: 0 60rpx;

		.head {
			display: flex;
			justify-content: center;
			flex-direction: column;
			height: 250rpx;
			color: #555555;

			text {
				line-height: 100rpx;
				font-size: 60rpx;
				font-weight: bold;
			}
		}

		.form-box {
			padding-top: 100rpx;

			.row-input {
				display: flex;
				flex-direction: column;
				border-bottom: 1rpx solid #e2e3eb;
			}

			.tip {
				height: 40rpx;
				padding-top: 30rpx;
				font-size: 32rpx;
				color: #e2e3eb;
			}

			.input-box {
				display: flex;
				justify-content: space-between;
				align-items: center;
				width: 600rpx;
				height: 100rpx;
				color: #8c8c8c;
				font-size: 40rpx;

				image {
					width: 40rpx;
					height: 40rpx;
				}

				.placeholder {
					color: #eaeaea;
					font-size: 35rpx;
				}
			}

			.login-btn {
				margin-top: 100rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				height: 100rpx;
				color: #FFFFFF;
				border-radius: 50rpx;
				font-size: 35rpx;
				background: linear-gradient(to right, #5633e3, #6368ea);
				box-shadow: 0rpx 10rpx 20rpx 0rpx rgba(98, 92, 236, 0.4);
			}
		}

		.menu-link {
			position: fixed;
			width: 750rpx;
			left: 0;
			bottom: 50rpx;
			display: flex;
			justify-content: center;
			font-size: 26rpx;
			color: #8c8c8c;
			font-weight: bold;

			text {
				margin-left: 20rpx;
				color: #555555;
			}
		}
	}
</style>
