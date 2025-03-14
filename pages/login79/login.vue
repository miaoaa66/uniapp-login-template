<template>
	<view class="container">
		<view class="bg bg-color-base margin-b20"></view>
		<view class="tab vs-row vs-align-center">
			<image class="tab-bg" src="./static/login79_bg.png" mode=""></image>
			<view class="vs-row vs-align-center">
				<view class="vs-column vs-align-center margin-r40" @click="cur = 0"> <text class="font-50 margin-b20"
						:class="[cur ? 'color-black-3': 'color-base']">登录</text>
					<view class="line bg-color-base" v-if="!cur"></view>
				</view>
				<view class="vs-column vs-align-center" @click="cur = 1"> <text class="font-46 margin-b20"
						:class="[cur ? 'color-base': 'color-black-3']">注册</text>
					<view class="line bg-color-base" v-if="cur"></view>
				</view>
			</view>
		</view>
		<view class="login margin-b80" v-if="!cur">
			<view class="input vs-row vs-align-center margin-b40">
				<image class="input-icon margin-r20" src="./static/login79_account.png" mode=""></image>
				<input class="vs-flex-item color-base font-30" type="text" value="" :maxlength="11"
					placeholder="请输入您的手机号/邮箱" placeholder-class="input-placeholder" />
			</view>
			<view class="input vs-row vs-align-center margin-b40">
				<image class="input-icon margin-r20" src="./static/login79_password.png" mode=""></image> <input
					class="vs-flex-item color-base font-30" type="text" password value="" placeholder="请输入您的登录密码"
					placeholder-class="input-placeholder" />
			</view>
		</view>
		<view class="register margin-b80" v-if="cur">
			<view class="input vs-row vs-align-center margin-b40">
				<image class="input-icon margin-r20" src="./static/login79_account.png" mode=""></image>
				<input class="vs-flex-item color-base font-30" type="text" value="" :maxlength="11"
					placeholder="请输入您的手机号/邮箱" placeholder-class="input-placeholder" />
			</view>
			<view class="input vs-row vs-align-center margin-b40">
				<image class="input-icon margin-r20" src="./static/login79_password.png" mode=""></image> <input
					class="vs-flex-item color-base font-30" type="text" password value="" placeholder="请输入您的登录密码"
					placeholder-class="input-placeholder" />
			</view>
		</view>
		<view class="button bg-color-base vs-row vs-align-center vs-space-center margin-b20">
			<text class="font-34">立即{{ cur ? '注册': '登录' }}</text>
		</view>
		<view class="vs-row vs-align-center vs-space-center margin-b100">
			<text class="color-base font-28">忘记密码？</text>
		</view>
		<view class="other">
			<view class="vs-row vs-align-center margin-b40">
				<view class="separator vs-flex-item"></view> <text class="color-black-3 font-28">更多登录方式</text>
				<view class="separator vs-flex-item"></view>
			</view>
			<view class="other-items">
				<image class="other-icon" src="./static/login79_vx.png" mode=""></image>
				<image class="other-icon" src="./static/login79_wb.png" mode=""></image>
				<image class="other-icon" src="./static/login79_zfb.png" mode=""></image>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				cur: 0,
			}
		}
	}
</script>
<style lang="scss" scoped>
	.container {
		position: relative;
	}

	.bg {
		position: relative;
		width: 750rpx;
		height: 400rpx;
	}

	.tab {
		position: absolute;
		top: 250rpx;
		left: 20rpx;
		right: 20rpx;
		height: 150rpx;
		padding: 0 50rpx;
		background-color: #fff;
		border-top-left-radius: 20rpx;
		border-top-right-radius: 20rpx;

		&-bg {
			position: absolute;
			top: -200rpx;
			right: -20rpx;
			width: 440rpx;
			height: 285rpx;
		}
	}

	.line {
		width: 25rpx;
		height: 7rpx;
	}

	.login,
	.register {
		padding: 0 60rpx;
	}

	.input {
		width: 580rpx;
		height: 90rpx;
		padding: 0 30rpx;
		background-color: rgba(80, 100, 235, 0.1);
		border-radius: 100rpx;

		&-icon {
			width: 30rpx;
			height: 30rpx;
		}

		&-placeholder {
			color: #5064eb;
		}
	}

	.button {
		width: 630rpx;
		height: 90rpx;
		margin-left: 60rpx;
		border-radius: 100rpx;
		color: #fff;
	}

	.separator {
		height: 2rpx;
		margin: 0 30rpx;
		background-color: #f5f5f5;
	}

	.other {
		&-items {
			padding: 0 200rpx;
		}

		&-icon {
			width: 50rpx;
			height: 50rpx;
		}
	}

	.other-items {
		display: flex;
		justify-content: space-around;
		align-items: center;

		.other-icon {
			width: 60rpx;
			height: 60rpx;
		}
	}

	.margin-b10 {
		margin-bottom: 10rpx;
	}

	.margin-b20 {
		margin-bottom: 20rpx;
	}

	.margin-b40 {
		margin-bottom: 40rpx;
	}

	.margin-b80 {
		margin-bottom: 80rpx;
	}

	.margin-b100 {
		margin-bottom: 100rpx;
	}

	.margin-r20 {
		margin-right: 20rpx;
	}

	.margin-r40 {
		margin-right: 40rpx;
	}

	.font-28 {
		font-style: normal;
		font-size: 28rpx;
		font-family: Droid Sans Fallback;
	}

	.font-30 {
		font-style: normal;
		font-size: 30rpx;
		font-family: Droid Sans Fallback;
	}


	.font-34 {
		font-style: normal;
		font-size: 34rpx;
		font-family: Droid Sans Fallback;
	}

	.font-46 {
		font-style: normal;
		font-size: 46rpx;
		font-family: Droid Sans Fallback;
	}

	.font-50 {
		font-style: normal;
		font-size: 50rpx;
		font-family: Droid Sans Fallback;
	}

	.color-black-3 {
		color: #333;
	}

	.vs-space-center {
		justify-content: center;
	}


	.vs-row {
		flex-direction: row;
	}

	.vs-column {
		flex-direction: column;
	}

	.vs-align-center {
		display: flex;
		align-items: center;
	}

	.color-base {
		color: #5064eb;
	}

	.bg-color-base {
		background-color: #5064eb;
	}
</style>