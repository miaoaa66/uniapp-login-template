<template>
	<view class="content">
		<view class="title">登录</view>
		<view class="form-box">
			<view class="row-input">
				<input maxlength="16" placeholder="用户ID" />
			</view>
			<view class="row-input">
				<input maxlength="16" placeholder="密码" :password="passwordShow" />
				<image mode="aspectFill" @click="passwordShowHide" v-if="passwordShow"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login51/login51_password-hide.png">
				</image>
				<image mode="aspectFill" @click="passwordShowHide" v-else
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login51/login51_password-show.png">
				</image>
			</view>
			<view class="login-btn">登录</view>
			<view class="menu-link">
				<text @click="registerClick">新用户注册</text>
				<text>使用token登录</text>
			</view>
		</view>
		<view class="logo-box">
			<image mode="aspectFill"
				src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login51/login51_logo.png">
			</image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				passwordShow: true
			}
		},
		methods: {
			registerClick() {
				uni.navigateTo({
					url: "/pages/login51/register"
				})
			},

			passwordShowHide() {
				this.passwordShow = !this.passwordShow
			}
		}
	}
</script>

<style lang="scss">
	.title {
		line-height: 100rpx;
		font-size: 50rpx;
		font-weight: bold;
		padding-left: 50rpx;
		padding-top: 50rpx;
	}

	.head {
		display: flex;
		justify-content: center;
		align-items: flex-end;
		width: 100%;
		height: 35vh;

		image {
			width: 540rpx;
			height: 400rpx;
		}
	}

	.form-box {
		padding: 100rpx 80rpx 0rpx 80rpx;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 100rpx;
			margin: 40rpx 0rpx;
			padding: 0 20rpx;
			border-radius: 15rpx;
			border: 1rpx solid #a2a2a2;

			input {
				flex: 1;
			}

			image {
				margin: 020rpx;
				width: 40rpx;
				height: 40rpx;
				flex-grow: 0;
			}
		}

		.login-btn {
			height: 100rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			border-radius: 15rpx;
			font-size: 32rpx;
			letter-spacing: 5rpx;
			color: #e0e7eb;
			background-color: #2e74d6;
		}

		.menu-link {
			display: flex;
			justify-content: space-between;
			height: 100rpx;
			align-items: center;
			font-size: 26rpx;
			color: #2e74d6;
		}
	}

	.logo-box {
		padding-top: 60rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		height: 200rpx;

		image {
			width: 400rpx;
			height: 160rpx;
		}
	}
</style>