<template>
	<view class="main">
		<view class="box flip-card__front" :animation="animationMain">
			<text class="switch" @click='rotateFn(0)'>Signup</text>
			<view class="title">Log in</view>
			<view class="flip-card__form">
				<input class="flip-card__input" placeholder="Email" maxlength="18">
				<input class="flip-card__input" placeholder="Password" maxlength="18">
				<button class="flip-card__btn">Let`s go!</button>

			</view>
		</view>
		<view class="box flip-card__back back" :animation="animationBack">
			<text class="switch" @click='rotateFn(1)'>Login</text>
			<view class="title">Sign up</view>
			<view class="flip-card__form">
				<input class="flip-card__input" placeholder="Name" maxlength="18">
				<input class="flip-card__input" placeholder="Email" maxlength="18">
				<input class="flip-card__input" placeholder="Password" maxlength="18">
				<button class="flip-card__btn">Confirm!</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				animationMain: null, //正
				animationBack: null, //反
			};
		},
		methods: {
			// 点击切换
			rotateFn(type) {
				// 创建动画
				this.animation_main = uni.createAnimation({
					duration: 400,
					timingFunction: 'linear'
				})
				this.animation_back = uni.createAnimation({
					duration: 400,
					timingFunction: 'linear'
				})
				// 翻转角度
				this.animation_main.rotateY(type == 0 ? 180 : 0).step()
				this.animation_back.rotateY(type == 0 ? 0 : -180).step()
				this.animationMain = this.animation_main.export()
				this.animationBack = this.animation_back.export()
			},
		}
	}
</script>

<style lang="scss">
	.main {
		position: absolute;
		top: 40%;
		left: 50%;
		width: 650rpx;
		height: 800rpx;
		transform: translate(-50%, -50%);
		-webkit-perspective: 1000;
		-moz-perspective: 1000;
	}

	.box {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		backface-visibility: hidden;
	}

	.flip-card__front,
	.flip-card__back {
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
		-webkit-backface-visibility: hidden;
		backface-visibility: hidden;
		background: lightgrey;
		gap: 40rpx;
		border-radius: 10rpx;
		border: 4rpx solid #323232;
		box-shadow: 8rpx 8rpx #323232;
	}

	.back {
		width: 100%;
		transform: rotateY(-180deg);
	}

	.switch {
		position: absolute;
		top: 20rpx;
		right: 20rpx;
		font-size: 28rpx;
		text-decoration: underline;
		color: #0055ff;
	}

	.flip-card__form {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 40rpx;
	}

	.title {
		margin: 50rpx 0;
		font-size: 50rpx;
		font-weight: 900;
		text-align: center;
		color: #323232;
	}

	.flip-card__input {
		width: 500rpx;
		height: 80rpx;
		border-radius: 10rpx;
		border: 4rpx solid #323232;
		background-color: #fff;
		box-shadow: 8rpx 8rpx #323232;
		font-size: 30rpx;
		font-weight: 600;
		color: #323232;
		padding: 10rpx 20rpx;
		outline: none;
	}

	.flip-card__btn {
		margin: 30rpx 0;
		width: 240rpx;
		height: 80rpx;
		border-radius: 10rpx;
		border: 4rpx solid #323232;
		background-color: #fff;
		box-shadow: 8rpx 8rpx #323232;
		font-size: 34rpx;
		font-weight: 600;
		color: #323232;
		cursor: pointer;
	}

	.flip-card__btn:active,
	.button-confirm:active {
		box-shadow: 0rpx 0rpx #323232;
		transform: translate(6rpx, 6rpx);
	}
</style>