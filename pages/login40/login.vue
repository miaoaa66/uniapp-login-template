<template>
	<view>
		<view class="login-img">
			<view class="login" :class="hideEye?'password':''">
				<view class="hand"></view>
				<view class="hand hand-r"></view>
				<view class="arms">
					<view class="arm"></view>
					<view class="arm arm-r"></view>
				</view>
			</view>
		</view>
		<view class="login-form">
			<view class="form-input">
				<input type="text" value="" placeholder="用户名" maxlength="18" />
			</view>
			<view class="form-input">
				<input type="password" value="" maxlength="18" placeholder="密码" @focus="change" @blur="change" />
			</view>
			<view class="login-btn">登录</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				hideEye: false,
			}
		},
		methods: {
			change() {
				this.hideEye = !this.hideEye;
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #4b4d4b;
	}

	.login-form {
		height: auto;
		border-radius: 20rpx;
		background-color: #84593f;
		box-shadow: 0 4rpx 20rpx #c1c1c1;
		padding: 40rpx;
		margin: 0 40rpx;

		.login-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 80rpx;
			font-size: 30rpx;
			border-radius: 10rpx;
			background-color: #00aa00;
			color: #ffffff;
		}
	}

	.form-input input {
		background: #ffffff;
		border-radius: 10rpx;
		height: 80rpx;
		margin: 40rpx 0;
		padding: 0 20rpx;
		font-size: 30rpx;
	}

	.login-img {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 180rpx;
		margin-top: 300rpx;
	}

	.login {
		width: 422rpx;
		height: 216rpx;
		background-image: url("./static/login40_head.png");
		background-repeat: no-repeat;
		background-size: 100%;
		position: absolute;

		.hand {
			width: 68rpx;
			height: 68rpx;
			-webkit-border-radius: 80rpx;
			border-radius: 80rpx;
			background-color: #472d20;
			transform: scaleY(0.6);
			transition: 0.3s ease-out;
			position: absolute;
			left: 28rpx;
			bottom: -18rpx;
		}
	}

	.login .hand.hand-r {
		left: 340rpx;
	}

	.login .arms {
		top: 116rpx;
		position: absolute;
		width: 100%;
		height: 80rpx;
		overflow: hidden;
	}

	.login .arms .arm {
		width: 80rpx;
		height: 130rpx;
		position: absolute;
		left: 40rpx;
		top: 80rpx;
		background-image: url("./static/login40_hand.png");
		background-size: 100%;
		transition: 0.3s ease-out;
		transform: rotate(-20deg);
	}

	.login .arms .arm.arm-r {
		transform: rotate(20deg) scaleX(-1);
		left: 320rpx;
	}

	.login.password .hand {
		transform: translateX(84rpx) translateY(-30rpx) scale(0.7);
	}

	.login.password .hand.hand-r {
		transform: translateX(-82rpx) translateY(-30rpx) scale(0.7);
	}

	.login.password .arms .arm {
		transform: translateY(-80rpx) translateX(80rpx);
	}

	.login.password .arms .arm.arm-r {
		transform: translateY(-80rpx) translateX(-80rpx) scaleX(-1);
	}
</style>