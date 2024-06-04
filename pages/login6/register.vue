<template>
	<view class="content">
		<view class="head">
			<image mode="aspectFit" src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login6/login6_logo.png"></image>
		</view>
		<view class="form-box">
			<view class="row-input">
				<input placeholder="昵称" maxlength="18" />
			</view>
			<view class="row-input">
				<input placeholder="手机号" maxlength="11" type="number" />
			</view>
			<view class="row-input">
				<input placeholder="验证码" maxlength="6" />
				<view class="code">
					<text @click="getCode">获取验证码</text>
				</view>
			</view>
			<view class="row-input">
				<input placeholder="密码" password maxlength="18" />
			</view>
			<view class="register-btn">注册</view>
			<view class="menu-link">
				<text @click="goToLogin">返回登录</text>
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
			goToLogin() {
				uni.navigateTo({
					url: '/pages/login6/login'
				})
			},

			// 获取验证码
			getCode() {
				uni.showToast({
					title: "获取验证码"
				})
			}
		}
	}
</script>

<style lang="scss">
	.head {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 32vh;

		image {
			width: 360rpx;
			height: 360rpx;
		}
	}

	.form-box {
		padding: 0rpx 80rpx;

		.row-input {
			display: flex;
			justify-content: space-between;
			align-items: center;
			height: 120rpx;
			border-bottom: 1rpx solid #ececec;

			.code {
				padding-right: 30rpx;
				display: flex;
				justify-content: flex-end;
				align-items: center;
				width: 200rpx;
				font-size: 26rpx;
			}
		}

		.register-btn {
			margin-top: 30rpx;
			width: 100%;
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 30rpx;
			height: 100rpx;
			background-color: #393a3e;
			letter-spacing: 10rpx;
			color: #f1f1f3;
		}

		.menu-link {
			line-height: 100rpx;
			font-size: 28rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			color: #676769;
		}
	}
</style>
