<template>
	<view class="content">
		<view class="bg-upper">
			<image mode="aspectFill" src="./static/login32_logo.png"></image>
		</view>
		<view class="bg-down"></view>
		<view class="form-box">
			<view :class="type==1?'top-box':'top-box-selected'">
				<view class="top-left"></view>
				<view class="top-left2" @click="topClick">登录</view>
				<view class="top-right"></view>
				<view class="top-right2" @click="topClick">注册</view>
			</view>
			<view class="form">
				<view :class="accountSelected?'item-box selected-border':'item-box'">
					<image v-show="!accountSelected"
						src="./static/login32_account1.png.png">
					</image>
					<image v-show="accountSelected"
						src="./static/login32_account2.png.png">
					</image>
					<input maxlength="18" placeholder="请输入用户名" placeholder-class="input-placeholder"
						@focus="accountFocus" @blur="accountBlur" />
					<image
						src="./static/login32_close.png.png">
					</image>
				</view>
				<view :class="passwordSelected?'item-box selected-border':'item-box'">
					<image v-show="!passwordSelected"
						src="./static/login32_password1.png.png">
					</image>
					<image v-show="passwordSelected"
						src="./static/login32_password2.png.png">
					</image>
					<input maxlength="18" placeholder="请输入密码" placeholder-class="input-placeholder"
						@focus="passwordFocus" @blur="passwordBlur" />
				</view>
				<view class="login-btn">{{type==1?'立即登录':'立即注册'}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				type: 1, //1:登录  2:注册
				accountSelected: false,
				passwordSelected: false
			}
		},
		onLoad() {

		},
		methods: {
			// 账号输入框聚焦事件
			accountFocus(e) {
				this.accountSelected = true;
			},

			// 账号输入框失去焦点事件
			accountBlur(e) {
				this.accountSelected = false;
			},

			// 密码输入框聚焦事件
			passwordFocus(e) {
				this.passwordSelected = true;
			},

			// 密码输入框失去焦点事件
			passwordBlur(e) {
				this.passwordSelected = false;
			},

			// 头部点击
			topClick() {
				this.type = this.type == 1 ? 2 : 1
			}

		}
	}
</script>

<style lang="scss" scoped>
	.left-box {
		display: flex;
		justify-content: center;
		align-items: center;
		letter-spacing: 2rpx;
	}

	.right-box {
		position: absolute;
		right: 0;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.content {
		.bg-upper {
			display: flex;
			justify-content: center;
			align-items: flex-start;
			width: 100%;
			height: 50vh;
			background-color: #f3f1fe;
		}

		.bg-down {
			width: 100%;
			height: 50vh;
			background-color: #fff;
		}
	}

	.form-box {
		position: fixed;
		top: 30vh;
		left: 50rpx;
		width: 650rpx;

		.top {
			position: relative;
			height: 120rpx;
			display: flex;
			justify-content: center;
		}

		.top-box {
			@extend .top;

			.top-left {
				@extend .left-box;
				position: absolute;
				left: 0;
				top: 60rpx;
				width: 70%;
				height: 50%;
				background-color: #f7f6fb;
				z-index: 6;
			}

			.top-left2 {
				@extend .left-box;
				position: absolute;
				left: 0;
				top: 0;
				width: 43.3%;
				height: 100%;
				color: #362ca2;
				background-color: #f7f6fb;
				border-top-left-radius: 50rpx;
				border-top-right-radius: 80rpx;
				z-index: 8;
			}

			.top-right {
				@extend .right-box;
				top: 0;
				width: 63.3%;
				height: 50%;
				background-color: #dcd5ff;
				border-top-left-radius: 36rpx;
				border-top-right-radius: 50rpx;
				z-index: 7;
			}

			.top-right2 {
				@extend .right-box;
				top: 0;
				width: 58.8%;
				height: 100%;
				color: #fff;
				letter-spacing: 2rpx;
				background: radial-gradient(circle at center, #dbaafe, #dcd5ff);
				border-top-right-radius: 50rpx;
				border-bottom-left-radius: 100rpx;
				z-index: 8;
			}
		}

		.top-box-selected {
			@extend .top;

			.top-left {
				@extend .left-box;
				position: absolute;
				left: 0;
				top: 0;
				width: 46.6%;
				height: 50%;
				background-color: #dccdff;
				z-index: 9;
				border-top-left-radius: 50rpx;
				border-top-right-radius: 36rpx;
			}

			.top-left2 {
				@extend .left-box;
				position: absolute;
				left: 0;
				top: 0;
				width: 41.6%;
				height: 100%;
				color: #fff;
				background: radial-gradient(circle at center, #dbaafe, #dcd5ff);
				border-top-left-radius: 50rpx;
				border-top-right-radius: 36rpx;
				border-bottom-right-radius: 80rpx;
				z-index: 10;
			}

			.top-right {
				@extend .right-box;
				top: 60rpx;
				width: 70%;
				height: 50%;
				background-color: #f7f6fb;
				border-top-right-radius: 50rpx;
				z-index: 9;
			}

			.top-right2 {
				@extend .right-box;
				top: 0;
				width: 59.1%;
				height: 100%;
				color: #362ca2;
				letter-spacing: 2rpx;
				background-color: #f7f6fb;
				border-top-left-radius: 80rpx;
				border-top-right-radius: 50rpx;
				z-index: 11;
			}
		}

		.form {
			padding-top: 40rpx;
			width: 650rpx;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			border-bottom-left-radius: 50rpx;
			border-bottom-right-radius: 50rpx;
			background-color: #f7f6fb;
			box-shadow: rgba(220, 205, 255, 1.0) 0rpx 4rpx 8rpx 0rpx;


			.item-box {
				display: flex;
				justify-content: flex-start;
				align-items: center;
				width: 100%;
				height: 120rpx;
				border-bottom: 1rpx solid #E5E5E5;

				image {
					width: 30rpx;
					height: 30rpx;
					padding: 0 40rpx;
					flex-shrink: 0;
				}

				input {
					flex: 1;
				}

				.input-placeholder {
					font-size: 30rpx;
					color: #C3C3C5;
				}
			}

			.selected-border {
				border-bottom: 1rpx solid #A099D4;
			}

			.login-btn {
				margin-top: 80rpx;
				margin-bottom: 60rpx;
				width: 240rpx;
				height: 65rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				background-color: #362ca2;
				color: #fff;
				font-size: 28rpx;
				letter-spacing: 2rpx;
				border-radius: 50rpx;
			}
		}
	}
</style>