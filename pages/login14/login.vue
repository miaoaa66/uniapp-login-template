<template>
	<view class="content">
		<view class="tab-box">
			<view class="title" :class="tabsIndex==0?'tabs-selected':'tabs-no-selected'" @click="tabsClick(0)">手机登录
			</view>
			<view class="title" :class="tabsIndex==1?'tabs-selected':'tabs-no-selected'" @click="tabsClick(1)">账号登录
			</view>
		</view>
		<view class="name">手机号</view>
		<view class="row-input">
			<input placeholder="请输入手机号" maxlength="11" placeholder-class="placeholder-input" />
		</view>
		<view class="row-input">
			<input :placeholder="tabsIndex==0?'请输入验证码':'请输入密码'" :password="tabsIndex==0?false:true" maxlength="20" placeholder-class="placeholder-input" />
			<view v-if="tabsIndex==0" class="code">获取验证码</view>
		</view>
		<view class="login-btn">登录</view>
		<view class="agreement">注册即同意
			<text class="link">用户协议</text>和<text class="link">隐私政策</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabsIndex: 0
			}
		},
		methods: {
			tabsClick(index) {
				this.tabsIndex = index
			}
		}
	}
</script>

<style lang="scss" scoped>

	.content {
		padding: 200rpx 60rpx;

		.tab-box {
			display: flex;
			align-items: flex-end;

			.title {
				margin-right: 50rpx;
				height: 55rpx;
			}

			.tabs-no-selected {
				font-size: 32rpx;
			}

			.tabs-selected {
				font-size: 40rpx;
				font-weight: bold;
				border-bottom: 6rpx solid #fe8d85;
			}
		}

		.name {
			padding-top: 100rpx;
			font-size: 26rpx;
		}

		.row-input {
			display: flex;
			justify-content: space-between;
			height: 120rpx;
			border-bottom: 1rpx solid #dfdfdf;

			.placeholder-input {
				font-size: 28rpx;
				color: #c5c5c5;
			}

			input {
				height: 120rpx;
			}

			.code {
				display: flex;
				justify-content: center;
				align-items: center;
				color: #fe8d85;
				font-size: 26rpx;
			}
		}

		.login-btn {
			margin-top: 80rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 100rpx;
			background-color: #fe8d85;
			color: #FFFFFF;
			border-radius: 50rpx;
		}

		.agreement {
			display: flex;
			justify-content: center;
			letter-spacing: 5rpx;
			line-height: 100rpx;
			font-size: 24rpx;

			.link {
				color: #007AFF;
				text-decoration: underline;
			}
		}
	}
</style>
