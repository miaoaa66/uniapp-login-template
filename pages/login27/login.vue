<template>
	<view class="content">
		<view class="title">
			<text class="thing" style="width: 100rpx;"></text>
			<text class="name">用户登录</text>
			<text class="thing" style="width: 300rpx;"></text>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login27/login27_account.png">
				</image>
				<input placeholder="手机号码或邮箱" maxlength="18" />
			</view>
			<view class="row-input">
				<image
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login27/login27_password.png">
				</image>
				<input placeholder="登陆密码" :password="!isShowPassword" />
				<image v-if="isShowPassword" @click="showOrHide"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login27/login27_password_show.png">
				</image>
				<image v-else @click="showOrHide"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login27/login27_password_hide.png">
				</image>
			</view>
			<view class="menu-link">
				<text @click="forget">找回密码</text>
			</view>
			<view class="login-btn">登录</view>
			<view class="tip-link">
				还没有账号?<text @click="register">注册</text>
			</view>
		</view>
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
			// 密码显示/密码隐藏
			showOrHide() {
				this.isShowPassword = !this.isShowPassword
			},

			// 找回密码
			forget() {
				uni.navigateTo({
					url: '/pages/login27/forget'
				})
			},
			
			// 注册
			register(){
				uni.navigateTo({
					url: '/pages/login27/register'
				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #0b0c14;
	}

	.content {
		padding: 100rpx 40rpx;
	}

	.title {
		display: flex;
		flex-direction: column;
		height: 200rpx;

		.thing {
			height: 8rpx;
			background-image: linear-gradient(to right, #3feaea, #0168e6);
		}

		.name {
			font-weight: bold;
			font-size: 50rpx;
			line-height: 120rpx;
			background: linear-gradient(to right, #3feaea, blue);
			background-clip: text;
			color: transparent;
			letter-spacing: 5rpx;
		}
	}

	.form-box {

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			height: 110rpx;
			background-color: #2b2f41;
			border-radius: 8rpx;
			margin-bottom: 50rpx;

			image {
				margin: 0 30rpx;
				flex-shrink: 0;
				width: 38rpx;
				height: 38rpx;
			}

			input {
				width: 460rpx;
				font-size: 30rpx;
				color: #a1a2a3;
			}
		}

		.menu-link {
			display: flex;
			justify-content: flex-end;
			align-items: center;
			font-size: 28rpx;
			color: #676c7f;
		}

		.login-btn {
			margin-top: 60rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 35rpx;
			letter-spacing: 15rpx;
			color: #FFFFFF;
			height: 110rpx;
			border-radius: 8rpx;
			background-image: linear-gradient(to right, #3feaea, #0168e6);
		}

		.tip-link {
			position: fixed;
			letter-spacing: 4rpx;
			bottom: 50rpx;
			left: 40rpx;
			font-size: 28rpx;
			color: #a1a2a3;

			text {
				padding-left: 10rpx;
				color: #226ef0;
			}
		}
	}
</style>
