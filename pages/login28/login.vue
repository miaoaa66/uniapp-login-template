<template>
	<view class="content">
		<view class="tab-box">
			<text class="cell-tab" @click="tabChange(0)" :class="tabIndex==0?'active':''">登录</text>
			<text class="cell-tab" @click="tabChange(1)" :class="tabIndex==1?'active':''">注册</text>
		</view>
		<view class=" head-bg">
			<image mode="heightFix" src="./static/login28_bg.png"></image>
		</view>
		<view class="form-box">
			<view class="row-input">
				<image src="./static/login28_account.png"></image>
				<input placeholder="用户名" maxlength="12" />
			</view>
			<view class="row-input" v-if="tabIndex==0?false:true">
				<image src="./static/login28_code.png"></image>
				<input placeholder="短信验证码" maxlength="6" />
				<view class="code">
					<text>获取验证码</text>
				</view>
			</view>
			<view class="row-input">
				<image src="./static/login28_password.png"></image>
				<input placeholder="密码" password maxlength="18" />
			</view>
		</view>
		<view class="login-btn">
			<text>{{tabIndex==0?'登录':'注册'}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabIndex: 0
			}
		},
		methods: {
			// tab切换
			tabChange(id) {
				this.tabIndex = id
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #222e3c;
	}

	.tab-box {
		padding-left: 40rpx;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		height: 100rpx;

		.cell-tab {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100rpx;
			font-size: 35rpx;
			color: #69727b;
		}

		.active {
			color: #a1a7ad;
			font-size: 40rpx;
		}
	}

	.head-bg {
		position: relative;
		display: flex;
		justify-content: center;
		align-items: flex-end;
		width: 100%;
		height: 30vh;

		image {
			width: 399rpx;
			height: 324rpx;
		}
	}

	.form-box {
		margin: 0 40rpx;
		border-radius: 15rpx;
		border: 1rpx solid #222427;
		box-sizing: border-box;
		background-color: #FFFFFF;

		.row-input {
			margin: 0 40rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;
			border-bottom: 1rpx solid #eaeaea;
			height: 120rpx;
			font-size: 30rpx;

			image {
				flex-shrink: 0;
				margin-right: 30rpx;
				width: 38rpx;
				height: 40rpx;
			}

			input {
				flex: 1;
				width: 400rpx;
				color: #818181;
			}

			.code {
				display: flex;
				justify-content: center;
				align-items: center;
				height: 60rpx;
				width: 180rpx;
				font-size: 26rpx;
				border-radius: 10rpx;
				border: 1rpx solid #eaeaea;
				color: #818181;
			}
		}
	}

	.login-btn {
		margin: 40rpx 80rpx;
		font-size: 30rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		height: 90rpx;
		border-radius: 10rpx;
		background-color: #54606e;
		color: #cbd0d6;
	}
</style>
