<template>
	<view class="screen">
		<view class="screen__content">
			<view class="login">
				<view class="login__field">
					<image class="login__icon" src="./static/account.png">
					</image>
					<input type="text" class="login__input" placeholder="User name / Email" maxlength="18">
				</view>
				<view class="login__field">
					<image class="login__icon" src="./static/password.png">
					</image>
					<input type=" password" class="login__input" placeholder="Password" maxlength="18">
				</view>
				<button class="button login__submit">
					<text class="button__text">LOG IN NOW</text>
					<image class="button__icon" src="./static/right_arrow.png">
					</image>
				</button>
			</view>
			<view class="social-login">
				<text>log in</text>
				<view class="social-icons">
					<image class="social-login__icon" src="./static/vx.png">
					</image>
					<image class="social-login__icon" src="./static/wb.png">
					</image>
					<image class="social-login__icon" src="./static/fs.png">
					</image>
				</view>
			</view>
		</view>
		<view class="screen__background">
			<text class="screen__background__shape screen__background__shape4"></text>
			<text class="screen__background__shape screen__background__shape3"></text>
			<text class="screen__background__shape screen__background__shape2"></text>
			<text class="screen__background__shape screen__background__shape1"></text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {};
		},
		methods: {}
	};
</script>

<style lang="scss" scoped>
	.screen {
		background: linear-gradient(90deg, #5D54A4, #7C78B8);
		position: relative;
		height: 100vh;
		width: 100vw;
	}

	.screen__content {
		z-index: 1;
		position: relative;
		height: 100%;
	}

	.screen__background {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: 0;
		-webkit-clip-path: inset(0 0 0 0);
		clip-path: inset(0 0 0 0);
	}

	.screen__background__shape {
		transform: rotate(45deg);
		position: absolute;
	}

	.screen__background__shape1 {
		height: 1040rpx;
		width: 1040rpx;
		background: #FFF;
		top: -100rpx;
		right: 240rpx;
		border-radius: 0 144rpx 0 0;
	}

	.screen__background__shape2 {
		height: 440rpx;
		width: 440rpx;
		background: #6C63AC;
		top: -344rpx;
		right: 0;
		border-radius: 64rpx;
	}

	.screen__background__shape3 {
		height: 1080rpx;
		width: 380rpx;
		background: linear-gradient(270deg, #5D54A4, #6A679E);
		top: -48rpx;
		right: 0;
		border-radius: 64rpx;
	}

	.screen__background__shape4 {
		height: 500rpx;
		width: 300rpx;
		background: #7E7BB9;
		bottom: 100rpx;
		right: -50rpx;
		border-radius: 120rpx;
	}

	.login {
		width: 640rpx;
		padding: 60rpx;
		padding-top: 312rpx;
	}

	.login__field {
		padding: 40rpx 0rpx;
		position: relative;
	}

	.login__icon {
		position: absolute;
		top: 60rpx;
		width: 35rpx;
		height: 35rpx;
		color: #7875B5;
	}

	.login__input {
		border: none;
		border-bottom: 4rpx solid #D1D1D4;
		background: none;
		padding: 20rpx;
		padding-left: 48rpx;
		font-weight: 700;
		width: 60%;
		transition: .2s;
	}

	.login__input:active,
	.login__input:focus,
	.login__input:hover {
		outline: none;
		border-bottom-color: #6A679E;
	}

	.login__submit {
		background: #fff;
		font-size: 28rpx;
		margin-top: 60rpx;
		padding: 32rpx 40rpx;
		border-radius: 32rpx;
		border: 2rpx solid #D4D3E8;
		text-transform: uppercase;
		font-weight: 700;
		display: flex;
		align-items: center;
		width: 100%;
		color: #4C489D;
		box-shadow: 0px 4rpx 4rpx #5C5696;
		cursor: pointer;
		transition: .2s;
	}

	.login__submit:active,
	.login__submit:focus,
	.login__submit:hover {
		border-color: #6A679E;
		outline: none;
	}

	.button__icon {
		width: 80rpx;
		height: 80rpx;
		margin-left: auto;
	}

	.social-login {
		position: absolute;
		height: 280rpx;
		width: 340rpx;
		text-align: center;
		bottom: 0rpx;
		right: 0rpx;
		color: #fff;
	}

	.social-icons {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.social-login__icon {
		width: 45rpx;
		height: 45rpx;
		padding: 20rpx;
		text-shadow: 0rpx 0rpx 16rpx #7875B5;
	}
</style>