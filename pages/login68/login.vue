<template>
	<view class="content">
		<view class="head">
			<text>欢迎登录系统</text>
		</view>
		<view class="form-box">
			<view class="row-input">
				<text class="tip">账号</text>
				<view class="input-box">
					<input v-model="phone" maxlength="11" placeholder="手机号/用户名/邮箱" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="row-input">
				<text class="tip">密码</text>
				<view class="input-box">
					<input v-model="password" maxlength="18" password placeholder="请输入密码"
						placeholder-class="placeholder" />
				</view>
			</view>
			<view class="login-btn">
				登录
			</view>
			<view class="register-btn">
				注册
			</view>
			<view class="menu-link">
				<text>忘记密码</text>
				<view class="line"></view>
				<text>免密码登录</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				phone: "",
				password: ""
			}
		},
		methods: {}
	}
</script>

<style lang="scss">
	page {
		background-color: #121325;
	}

	.content {
		padding: 0 80rpx;

		.head {
			display: flex;
			justify-content: flex-end;
			flex-direction: column;
			height: 200rpx;
			color: #FFFFFF;

			text {
				line-height: 100rpx;
				font-size: 60rpx;
			}
		}

		.form-box {
			padding-top: 100rpx;

			.row-input {
				display: flex;
				flex-direction: column;
				border-bottom: 2rpx solid #292d4a;
			}

			.tip {
				height: 40rpx;
				padding-top: 30rpx;
				font-size: 36rpx;
				letter-spacing: 2rpx;
				color: #ffffff;
			}

			.input-box {
				display: flex;
				justify-content: space-between;
				align-items: center;
				width: 600rpx;
				height: 100rpx;
				color: #ffffff;
				font-size: 30rpx;

				.placeholder {
					color: #3f414e;
					font-size: 30rpx;
				}
			}

			.login-btn {
				margin-top: 100rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				height: 130rpx;
				color: #FFFFFF;
				border-radius: 100rpx;
				font-size: 30rpx;
				background-color: #007aff;
			}

			.register-btn {
				margin-top: 40rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				height: 130rpx;
				color: #ffffff;
				border-radius: 100rpx;
				font-size: 35rpx;
				background-color: #2a2c3b;
			}

			.menu-link {
				margin-top: 60rpx;
				display: flex;
				justify-content: center;
				align-items: center;

				text {
					width: 200rpx;
					color: #ffffff;
					font-size: 28rpx;
					text-align: center;
				}
				
				.line{
					width: 4rpx;
					height: 20rpx;
					background-color: #ffffff;
				}
			}
		}
	}
</style>