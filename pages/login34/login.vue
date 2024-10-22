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
				color: #000;
			}

			.input-box {
				display: flex;
				justify-content: space-between;
				align-items: center;
				width: 600rpx;
				height: 100rpx;
				color: #8c8c8c;
				font-size: 40rpx;

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
				background-color: #3081ed;
			}
			
			.register-btn{
				margin-top: 40rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				height: 100rpx;
				color: #000;
				border-radius: 50rpx;
				font-size: 35rpx;
				background-color: #dddddd;
			}
		}
	}
</style>