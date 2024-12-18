<template>
	<view class="content">
		<view class="title">
			登录
		</view>
		<view class="row-input">
			<image src="./static/login24_email.png"></image>
			<input placeholder="邮件或用户名" maxlength="18" />
		</view>
		<view class="row-input">
			<image src="./static/login24_password.png"></image>
			<input placeholder="密码" maxlength="18" :password="!isShowPassword" />
			<image v-if="isShowPassword" @click="showOrHide" src="./static/login24_password_show.png"></image>
			<image v-else @click="showOrHide" src="./static/login24_password_hide.png"></image>
		</view>
		<view class="menu-link">
			<text @click="register">没有账号?</text>
		</view>
		<view class="login-btn">登录</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isShowPassword: true,
			}
		},
		methods: {
			// 显示密码/隐藏密码
			showOrHide() {
				this.isShowPassword = !this.isShowPassword
			},

			// 注册
			register() {
				uni.navigateTo({
					url: '/pages/login24/register'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		padding: 0 60rpx;
	}

	.title {
		padding-bottom: 50rpx;
		height: 200rpx;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		font-size: 60rpx;
		letter-spacing: 5rpx;
		color: #585858;
	}

	.row-input {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		height: 110rpx;
		border-bottom: 1rpx solid #ededed;

		image {
			margin: 0 20rpx;
			flex-shrink: 0;
			width: 36rpx;
			height: 40rpx;
		}

		input {
			width: 480rpx;
			font-size: 30rpx;
		}
	}

	.menu-link {
		display: flex;
		justify-content: flex-end;
		line-height: 100rpx;
		font-size: 28rpx;
		color: #6a75cf;
		padding-bottom: 20rpx;
	}

	.login-btn {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		color: #FFFFFF;
		font-size: 38rpx;
		letter-spacing: 5rpx;
		height: 110rpx;
		border-radius: 20rpx;
		background-color: #4772fb;
	}
</style>
