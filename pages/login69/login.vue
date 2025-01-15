<template>
	<view class="content">
		<view class="login-box">
			<view class="form-box">
				<view class="title">登录</view>
				<view class="input-row">
					<input maxlength="14" placeholder="电子邮箱" />
				</view>
				<view class="input-row">
					<input maxlength="18" placeholder="密码" password />
				</view>
				<view class="menu-link">
					<text>忘记密码?</text>
				</view>
			</view>
			<view class="triangle"></view>
			<view class="login-btn">→</view>
		</view>
		<view class="register">
			<text>没有账号？立即注册</text>
		</view>
	</view>
</template>

<script>
</script>

<style lang="scss" scoped>
	page {
		background-color: #fe1840;
	}

	.login-box {
		margin: 160rpx auto;
		position: relative;
		width: 622rpx;
		height: 620rpx;
		background-color: #ffffff;
		border-radius: 60rpx;

		.form-box {
			display: flex;
			justify-content: center;
			align-items: center;
			flex-direction: column;

			.title {
				display: flex;
				justify-content: center;
				align-items: center;
				font-size: 46rpx;
				letter-spacing: 4rpx;
				font-weight: bold;
				height: 160rpx;
				color: #fe1840;
			}

			.input-row {
				margin: 40rpx 0;
				width: 500rpx;
				height: 100rpx;
				border-bottom: 2rpx solid #808080;

				input {
					font-size: 30rpx;
				}
			}

			.menu-link {
				color: #808080;
				font-size: 30rpx;
			}
		}
	}
	
	.register {
		padding-top: 180rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 34rpx;
		font-weight: bold;
		color: #ffffff;
	}

	.triangle {
		position: absolute;
		top: 346rpx;
		left: -125rpx;
		width: 800rpx;
		height: 800rpx;
		border-radius: 20%;
		transform: rotate(60deg) skewY(30deg) scaleX(0.866);
		overflow: hidden;
		z-index: -1;
	}

	.triangle:before,
	.triangle:after {
		content: '';
		position: absolute;
		width: 800rpx;
		height: 800rpx;
		background-color: #ffffff;
		border-radius: 20% 20% 20% 55%;
		transform: scaleX(1.155) skewY(-30deg) rotate(-30deg) translateY(-42.3%) skewX(30deg) scaleY(0.866) translateX(-24%);
	}

	.triangle:after {
		border-radius: 20% 20% 55% 20%;
		background: #ffffff;
		transform: scaleX(1.155) skewY(-30deg) rotate(-30deg) translateY(-42.3%) skewX(-30deg) scaleY(0.866) translateX(24%);
	}

	.login-btn {
		position: absolute;
		top: 660rpx;
		right: 40rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		width: 140rpx;
		height: 140rpx;
		background-color: #fff;
		border-radius: 100rpx;
		font-size: 60rpx;
		font-weight: bold;
		color: #fe1840;
	}
</style>