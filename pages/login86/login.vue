<template>
	<view class="container">
		<view class="form">
			<view class="sling-box">
				<view class="outer-ring">
				</view>
			</view>
			<view class="line"></view>
			<view class="title">后台登录管理系统</view>
			<input class="input-item" type="text" maxlength="20" placeholder="请输入邮箱账号" />
			<input class="input-item" type="password" maxlength="20" placeholder="请输入密码" />
			<input class="input-item" type="text" maxlength="20" placeholder="请填写真实姓名" />
			<input class="input-item" type="text" maxlength="20" placeholder="请输入身份证号码" />
			<input class="input-item" type="text" maxlength="20" placeholder="请输入手机号" />
			<view class="register-btn">注册</view>
		</view>
	</view>
</template>

<style lang="scss" scoped>
	page {
		background-color: #129b6f;
	}

	.title {
		text-align: center;
		font-size: 40rpx;
		font-weight: bold;
		letter-spacing: 2rpx;
		line-height: 100rpx;
	}

	.form {
		position: relative;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		margin: 280rpx 40rpx 0 40rpx;
		background-color: #ffffff;
		padding: 40rpx;
		border-radius: 4rpx;
		box-shadow: 0 0 4rpx rgba(0, 0, 0, 0.3);
	}

	.sling-box {
		position: absolute;
		display: flex;
		justify-content: center;
		width: 140rpx;
		height: 200rpx;
		border-top-left-radius: 20rpx;
		border-top-right-radius: 20rpx;
		background-color: rgba(241, 242, 244, 0.65);
		box-shadow: 0 0 4rpx rgba(0, 0, 0, 0.3);
		top: -160rpx;
		z-index: 8;

		.outer-ring {
			margin-top: 60rpx;
			width: 40rpx;
			height: 40rpx;
			background-color: #139a6f;
			z-index: 9;
			border-radius: 100rpx;
			border: 10rpx solid #f4f4f5;
		}
	}

	.line {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 200rpx;
		height: 10rpx;
		border-radius: 50rpx;
		background-color: #129b6f;
		z-index: 9;
	}

	.input-item {
		width: 540rpx;
		height: 80rpx;
		margin-bottom: 20rpx;
		padding: 0 20rpx;
		border: 1px solid #ccc;
		border-radius: 5px;
		font-size: 30rpx;
	}

	.register-btn {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 580rpx;
		height: 100rpx;
		font-size: 40rpx;
		letter-spacing: 4rpx;
		border-radius: 5px;
		background-color: #129b6f;
		color: #ffffff;
	}
</style>