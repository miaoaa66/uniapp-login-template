<template>
	<view class="form">
		<text class="login-lable">Login</text>
		<input class="input" type="text" placeholder="UserName" maxlength="16">
		<input class="input" type="text" placeholder="PassWord" minlength="18">
		<view class="btn">Submit</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {};
		},
		methods: {},
	};
</script>

<style scoped lang='scss'>
	.form {
		margin: 200rpx auto;
		width: 550rpx;
		height: 650rpx;
		padding: 40rpx;
		border-radius: 15rpx;
		background: #e8e8e8;
		box-shadow: 24rpx 24rpx 36rpx #bababa,
			-24rpx -24rpx 36rpx #ffffff;
		display: flex;
		flex-direction: column;
		align-items: stretch;
		justify-content: space-evenly;
	}

	.login-lable {
		height: 100rpx;
		text-align: center;
		font-size: 70rpx;
		letter-spacing: 6rpx;
		font-weight: 600;
		text-shadow: -4rpx -4rpx 6rpx #ffffff,
			4rpx 4rpx 6rpx #bababa;
		position: relative;
		top: -28rpx;
	}

	.input {
		height: 100rpx;
		padding: 2rpx 16rpx;
		border: none;
		font-size: 32rpx;
		letter-spacing: 4rpx;
		border-radius: 16rpx;
		background: #f3f3f3;
		box-shadow: inset -4rpx -4rpx 8rpx #ffffff,
			inset 4rpx 4rpx 8rpx rgba(0, 0, 0, 0.356);
		text-decoration: none;
		background: linear-gradient(to right, white, #5e5e5e10, white);
		background-size: 200% 200%;
		animation: input 4s infinite;
	}

	.btn {
		display: flex;
		justify-content: center;
		align-items: center;
		letter-spacing: 6rpx;
		color: white;
		font-weight: 700;
		height: 100rpx;
		background: linear-gradient(144deg, #af40ff,
				#5b42f3 50%, #00ddeb);
		border-radius: 15rpx;
		border: none;
		box-shadow: inset 1px 3px 3px #ffffffbd,
			inset -4px -4px 3px #00000046;
		background-size: 150% 150%;
		animation: input 5s infinite;
		transition: all 900ms ease-in;
	}

	.btn:hover {
		position: relative;
		bottom: 3px;
		background: linear-gradient(144deg, #9706ff,
				#2f0fff 50%, #18f0ff);
	}

	@keyframes input {
		0% {
			background-position: 0% 50%;
		}

		50% {
			background-position: 100% 50%;
		}

		100% {
			background-position: 0% 50%;
		}
	}
</style>