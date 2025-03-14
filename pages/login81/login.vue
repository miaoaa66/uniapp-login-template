<template>
	<view class="content">
		<view class="loginBox">
			<h3>登录</h3>
			<view class="inputBox">
				<view class="ipt">
					<image src="./static/login81_account.png"></image>
					<input type="text" value="" placeholder="请输入账号" maxlength="18" />
				</view>
				<view class="ipt">
					<image src="./static/login81_password.png"></image>
					<input type="passsword" value="" placeholder="请输入密码" maxlength="18" />
				</view>
				<view class="forgetPwd">
					<span>忘记密码</span>
					<span>没有账号，去注册</span>
				</view>
				<button>登录</button>
			</view>
			<view class="tipbox">
				<view class="txt">
					—— 其他账号登录 ——
				</view>
				<view class="otherUser">
					<image src="./static/login81_vx.png"></image>
					<image src="./static/login81_qq.png"></image>
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
		background: linear-gradient(to bottom, #fabec2, #fb7da8, #f5b3d3);
	}

	.loginBox {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -60%);
		width: 90%;
		background-color: #fff;
		border-radius: 20rpx;
		padding: 60rpx;
		box-sizing: border-box;
	}

	h3 {
		color: rgb(247, 120, 172);
		font-size: 40rpx;
		font-weight: bold;
		letter-spacing: 6rpx;
		margin-bottom: 40rpx;
	}

	.ipt {
		height: 100rpx;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		margin-bottom: 20rpx;
		background-color: #f5f5f5;
		border-radius: 10rpx;
		padding-left: 10rpx;
	}

	.ipt input {
		margin-left: 20rpx;
		font-size: 28rpx;
	}

	.ipt image {
		width: 40rpx;
		height: 40rpx;
	}

	.forgetPwd {
		font-size: 26rpx;
		color: #b5b5b5;
		text-align: end;
		padding: 0 10rpx;
		display: flex;
		justify-content: space-between;
	}

	button {
		margin-top: 20rpx;
		line-height: 85rpx;
		text-align: center;
		background: linear-gradient(to right, rgb(255, 170, 127), rgb(247, 120, 172));
		border-radius: 40rpx;
		color: #fff;
		margin-top: 40rpx;
	}

	.tipbox {
		text-align: center;
		margin-top: 100rpx;
	}

	.txt {
		font-size: 28rpx;
		color: #cbcbcb;
	}

	.otherUser {
		margin-top: 30rpx;
		display: flex;
		justify-content: center;
	}

	.otherUser image {
		margin-left: 20rpx;
		width: 80rpx;
		height: 80rpx;
	}
</style>