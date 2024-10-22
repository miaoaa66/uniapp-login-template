<template>
	<view class="content">
		<view class="logo">
			<image mode="aspectFill"
				src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login50/login50_logo.png">
			</image>
		</view>
		<view class="top-switch">
			<view :class="[isLogin ? 'active' : '','login']" @click="switchClick(1)">登录</view>
			<view :class="[isLogin ? '' : 'active','register']" @click="switchClick(2)">注册</view>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image mode="aspectFill"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login50/login50_account.png">
				</image>
				<input type="text" placeholder="账号" maxlength="18" />
			</view>
			<view class="row-input">
				<image mode="aspectFill"
					src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login50/login50_password.png">
				</image>
				<input type="text" placeholder="密码" maxlength="6" />
			</view>
			<view class="btn-box">
				<view class="btn login">{{isLogin?'登录':'注册'}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isLogin: true,
			}
		},
		methods: {
			// 切换
			switchClick(type) {
				this.isLogin = type == 1 ? true : false
			},
		}
	}
</script>

<style lang="scss">
	page {
		background-image: linear-gradient(to right, #8139f2, #b23af1);
	}

	.logo {
		margin-top: 100rpx;
		display: flex;
		justify-content: center;
		align-items: center;

		image {
			width: 200rpx;
			height: 200rpx;
		}
	}

	.top-switch {
		display: flex;
		justify-content: center;
		align-items: center;
		margin: 100rpx auto;
		width: 480rpx;
		height: 90rpx;
		border-radius: 50rpx;
		background-color: #8139f2;
		background:rgba(17, 17, 17, 0.2);

		.login,
		.register {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 240rpx;
			height: 80rpx;
			font-size: 28rpx;
			border-radius: 50rpx;
			color: #f8ede4;
			font-weight: bold;
		}

		.active {
			background-color: #ffffff;
			color: #55557f;
			font-weight: bold;
		}
	}

	.form-box {
		position: relative;
		margin: 0 50rpx;
		padding: 50rpx 0 100rpx 0;
		border-radius: 20rpx;
		background-color: #FFFFFF;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			margin: 0rpx 60rpx;
			height: 140rpx;
			font-size: 30rpx;
			border-bottom: 1rpx solid #cbb7d3;

			input {
				padding: 0 20rpx;
			}

			image {
				width: 40rpx;
				height: 40rpx;
			}
		}

		.btn-box {
			position: absolute;
			left: 50rpx;
			bottom: -20rpx;
			width: 550rpx;
			height: 120rpx;

			.btn {
				margin: 40rpx 0rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				width: 100%;
				height: 100rpx;
				border-radius: 20rpx;
				font-size: 35rpx;
				letter-spacing: 5rpx;
			}

			.login {
				color: #FFFFFF;
				background-image: linear-gradient(to right, #b23af1, #8139f2);
			}
		}

	}
</style>