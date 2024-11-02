<template>
	<view class="content">
		<view class="head">
			<image mode="aspectFill" src="./static/login13_bg.png"></image>
		</view>
		<view class="form-box">
			<view class="tab-title">
				<view class="row-tab" @click="tabChange(0)" :class="tabIndex==0?'active':''"
					style="border-right: 1rpx solid #e1e1e1;">
					<text class="name">登录</text>
					<text v-if="tabIndex==0" class="thing"></text>
				</view>
				<view class="row-tab" @click="tabChange(1)" :class="tabIndex==1?'active':''"
					style=" border-left: 1rpx solid #e1e1e1;">
					<text class="name">注册</text>
					<text v-if="tabIndex==1" class="thing"></text>
				</view>
			</view>
			<view v-if="tabIndex==0">
				<view class="row-input">
					<image src="./static/login13_card.png"></image>
					<input placeholder="请输入身份证" maxlength="18" />
				</view>
				<view class="row-input">
					<image src="./static/login13_password.png"></image>
					<input placeholder="请输入密码" password maxlength="18" />
				</view>
				<view class="login-btn" style="margin-top: 50rpx;">登录</view>
				<text class="menu-link">忘记密码?</text>
			</view>
			<view v-else>
				<view class="row-input">
					<image src="./static/login13_name.png"></image>
					<input placeholder="请输入姓名" maxlength="12" />
				</view>
				<view class="row-input">
					<image src="./static/login13_phone.png"></image>
					<input placeholder="请输入手机号" maxlength="11" />
				</view>
				<view class="row-input">
					<image src="./static/login13_card.png"></image>
					<input placeholder="请输入身份证" maxlength="18" />
				</view>
				<view class="row-input">
					<image src="./static/login13_password.png"></image>
					<input placeholder="请设置密码" password maxlength="18" />
				</view>
				<view class="row-input">
					<image src="./static/login13_password.png"></image>
					<input placeholder="请确认密码" password maxlength="18" />
				</view>
				<view class="agreement">
					注册即代表您已同意
					<text>《注册与使用协议》</text>
				</view>
				<view class="login-btn" style="margin-top: 15rpx;">注册</view>
			</view>
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
			// 切换
			tabChange(id) {
				this.tabIndex = id
			}
		}
	}
</script>

<style lang="scss">
	.head {
		width: 100%;
		height: 35vh;

		image {
			width: 100%;
			height: 100%;
		}
	}

	.form-box {
		padding: 0 40rpx;

		.tab-title {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 120rpx;

			.row-tab {
				position: relative;
				display: flex;
				align-items: center;
				justify-content: center;
				flex-direction: column;
				font-size: 40rpx;
				height: 45rpx;
				letter-spacing: 3rpx;
				width: 200rpx;

				.thing {
					position: absolute;
					top: 60rpx;
					width: 120rpx;
					height: 8rpx;
					border-radius: 3rpx;
					background-color: #37a09f;
				}
			}

			.active {
				color: #2f7a81;
			}
		}

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			border: 1rpx solid #e0e0e0;
			height: 90rpx;
			margin: 20rpx 0rpx;
			border-radius: 10rpx;

			image {
				flex-shrink: 0;
				margin: 0rpx 30rpx;
				width: 40rpx;
				height: 40rpx;
			}

			input {
				width: 450rpx;
				padding-left: 40rpx;
				font-size: 30rpx;
			}
		}

		.login-btn {
			font-size: 35rpx;
			color: #FFFFFF;
			letter-spacing: 5rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			height: 102rpx;
			border-radius: 10rpx;
			background: linear-gradient(#5ebcaf, #349c9f);
		}
		
		.agreement{
			display: flex;
			justify-content: center;
			font-size: 26rpx;
			line-height: 40rpx;
			color: #929292;
			
			text{
				color: #519297;
			}
		}

		.menu-link {
			display: flex;
			justify-content: center;
			line-height: 100rpx;
			font-size: 30rpx;
			color: #9f9f9f;
		}
	}
</style>
