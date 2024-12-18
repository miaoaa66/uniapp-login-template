<template>
	<view class="content">
		<view class="title">
			用户, 欢迎！
		</view>
		<view class="row-type" :class="typeIndex==0?'active':''" @click="typeClick(0)">
			<image v-if="typeIndex==0"
				src="./static/login10_pgone1.png">
			</image>
			<image v-else
				src="./static/login10_pgone2.png">
			</image>
			<text>手机号注册</text>
		</view>
		<view class="row-type" :class="typeIndex==1?'active':''" @click="typeClick(1)">
			<image v-if="typeIndex==1"
				src="./static/login10_email1.png">
			</image>
			<image v-else
				src="./static/login10_email2.png">
			</image>
			<text>邮箱注册</text>
		</view>
		<text class="other" @click="gotoLogin">我直接登录</text>
		<view class="row-input">
			<input :placeholder="typeIndex==0?'手机号':'邮箱'" :maxlength="typeIndex==0?11:18"
				placeholder-style="color:#b0b3bf" />
			<image mode="aspectFit"
				src="./static/login10_pgone2.png">
			</image>
		</view>
		<view class="row-input">
			<input placeholder="验证码" maxlength="6" placeholder-style="color:#b0b3bf" />
			<image mode="aspectFit"
				src="./static/login10_tick.png">
			</image>
		</view>
		<view class="row-input">
			<input placeholder="密码" :password="showPassword" maxlength="18" placeholder-style="color:#b0b3bf" />
			<image mode="aspectFit" v-if="!showPassword" @click="passwordChange"
				src="./static/login10_password1.png">
			</image>
			<image mode="aspectFit" v-else @click="passwordChange"
				src="./static/login10_password2.png">
			</image>
		</view>
		<view class="agreement">
			<view>
				我同意
				<text class="link">《此协议》</text>
			</view>
			<checkbox :checked="false" style="transform: scale(0.7);" />
		</view>
		<view class="register-btn">
			注册
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				typeIndex: 0,
				showPassword: false,
			}
		},
		methods: {
			// 类型切换
			typeClick(id) {
				this.typeIndex = id
			},

			// 去登录
			gotoLogin() {
				uni.navigateTo({
					url: '/pages/login10/login'
				})
			},

			// 密码图标切换
			passwordChange() {
				this.showPassword = !this.showPassword
			}
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		padding: 0 40rpx;
	}

	.title {
		display: flex;
		justify-content: center;
		align-items: flex-end;
		padding-bottom: 20rpx;
		font-size: 50rpx;
		font-weight: bold;
		color: #4a4a4a;
	}

	.row-type {
		margin: 30rpx 0;
		padding: 0 60rpx;
		height: 110rpx;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		border: 1rpx solid #f6f6f6;
		font-size: 24rpx;
		border-radius: 25rpx;
		color: #dee1f2;

		image {
			width: 40rpx;
			height: 40rpx;
		}

		text {
			padding-left: 80rpx;
		}
	}

	.active {
		background-color: #7584cb;
	}

	.other {
		display: flex;
		justify-content: center;
		line-height: 100rpx;
		color: #b0b3bf;
		font-size: 26rpx;
	}

	.row-input {
		padding: 0 40rpx;
		margin: 30rpx 0;
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 110rpx;
		background-color: #f2f3f6;
		border-radius: 8rpx;
		font-size: 30rpx;

		image {
			width: 40rpx;
			height: 40rpx;
		}
	}

	.agreement {
		padding-left: 10rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 26rpx;
		font-weight: bold;
		color: #939393;

		text {
			color: #a6afda;
		}
	}

	.register-btn {
		margin-top: 50rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		color: #FFFFFF;
		height: 110rpx;
		background: linear-gradient(to right, #8d96fb, #7e86e1);
		letter-spacing: 5rpx;
		font-size: 30rpx;
		border-radius: 25rpx;
	}
</style>
