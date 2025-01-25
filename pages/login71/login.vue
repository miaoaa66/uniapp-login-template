<template>
	<view class="container">
		<view class="form_area">
			<view class="title">SIGN UP</view>
			<view class="form_group">
				<view class="sub_title" for="name">Name</view>
				<input placeholder="name" class="form_style" type="text" maxlength="12">
			</view>
			<view class="form_group">
				<view class="sub_title" for="email">Email</view>
				<input placeholder="email" id="email" class="form_style" type="email" maxlength="16">
			</view>
			<view class="form_group">
				<view class="sub_title" for="password">Password</view>
				<input placeholder="password" id="password" class="form_style" type="password"
					maxlength="18">
			</view>
			<button class="btn">SIGN UP</button>
			<view>Have an Account?
				<view class="link">Login Here!</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		onLoad() {},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.container {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
	}

	.form_area {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		background-color: #EDDCD9;
		border: 4rpx solid #264143;
		margin-top: 100rpx;
		border-radius: 40rpx;
		box-shadow: 6rpx 8rpx 0rpx 2rpx #E99F4C;
	}

	.title {
		color: #264143;
		font-weight: bold;
		font-size: 40rpx;
		margin-top: 20rpx;
	}

	.sub_title {
		font-weight: bold;
		margin: 10rpx 0;
	}

	.form_group {
		display: flex;
		flex-direction: column;
		align-items: baseline;
		margin: 20rpx;
	}

	.form_style {
		border: 4rpx solid #264143;
		box-shadow: 6rpx 8rpx 0rpx 2rpx #E99F4C;
		width: 580rpx;
		padding: 24rpx 20rpx;
		border-radius: 8rpx;
		font-size: 30rpx;
	}

	.btn {
		padding: 30rpx;
		margin: 50rpx 0rpx;
		width: 580rpx;
		font-size: 30rpx;
		background: #DE5499;
		border-radius: 20rpx;
		font-weight: bold;
		box-shadow: 6rpx 6rpx 0rpx 0rpx #E99F4C;
	}

	.link {
		display: flex;
		justify-content: center;
		font-weight: bold;
		color: #264143;
		padding: 10rpx;
	}
</style>