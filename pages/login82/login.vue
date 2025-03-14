<template>
	<view class="content">
		<view class="topBox">
			<h3>WELCOME</h3>
			<h3>欢迎使用某某应用</h3>
		</view>
		<view class="inputBox">
			<view class="ipt">
				<h4>手机号</h4>
				<input type="text" value="" placeholder="请输入手机号码" maxlength="18" />
			</view>
			<view class="ipt">
				<h4>密码</h4>
				<input type="text" value="" placeholder="请输入密码" maxlength="18" />
			</view>
			<button class="loginBtn">登录</button>
			<button class="registerBtn">注册</button>

			<view class="tipbox">
				<view class="txt">
					—— 其他账号登录 ——
				</view>
				<view class="otherUser">
					<image src="./static/login82_vx.png"></image>
					<image src="./static/login82_qq.png"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			}
		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.content {
		height: 100vh;
		background-color: #5461d2;
	}

	.topBox {
		font-size: 34rpx;
		color: #fff;
		padding: 50rpx 50rpx;
	}

	.inputBox {
		width: 750rpx;
		height: calc(100vh - 150rpx);
		background-color: #fff;
		border-top-left-radius: 40rpx;
		border-top-right-radius: 40rpx;
		padding: 60rpx;
		box-sizing: border-box;
	}

	.ipt {
		margin-bottom: 50rpx;
	}

	.ipt h4 {
		margin-bottom: 20rpx;
		font-size: 36rpx;
		color: #333;
	}

	.ipt input {
		border-bottom: 1px solid #dedede;
		padding-bottom: 20rpx;
		font-size: 28rpx;
	}

	.loginBtn {
		margin-top: 20rpx;
		line-height: 85rpx;
		text-align: center;
		background: linear-gradient(to right, rgb(86, 104, 214), rgb(86, 104, 214));
		border-radius: 40rpx;
		color: #fff;
		margin-top: 50rpx;
	}

	.registerBtn {
		margin-top: 20rpx;
		line-height: 85rpx;
		text-align: center;
		border-radius: 40rpx;
		color: rgb(86, 104, 214);
		margin-top: 50rpx;
		border: 1rpx solid #c8c8c8;
		background-color: #fff;
	}

	.tipbox {
		margin-top: 100rpx;
	}

	.otherUser {
		margin-top: 30rpx;
		display: flex;
		justify-content: center;
	}

	.txt {
		text-align: center;
		font-size: 28rpx;
		color: #969696;
	}

	.otherUser image {
		width: 80rpx;
		height: 70rpx;
		margin-left: 20rpx;
	}
</style>