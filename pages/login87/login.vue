<template>
	<view class="content">
		<view class="head">
			<text class="title">欢迎登录xxxx系统</text>
			<view class="tab-box">
				<view class="row-tab" @click="tabClick(0)">
					<text :class="tabIndex==0?'tab-active':''">登录</text>
					<text class="thing" v-show="tabIndex==0?true:false"></text>
				</view>
				<view class="row-tab" @click="tabClick(1)">
					<text :class="tabIndex==1?'tab-active':''">注册</text>
					<text class="thing" v-show="tabIndex==1?true:false"></text>
				</view>
			</view>
		</view>
		<view class="form-box">
			<view v-show="tabIndex==0">
				<view class="row-input">
					<view class="tip-box">
						<image src="./static/login87_phone.png"></image>
						<text>账号</text>
					</view>
					<input maxlength="11" placeholder="请输入账号" />
				</view>
				<view class="row-input">
					<view class="tip-box">
						<image src="./static/login87_password.png"></image>
						<text>密码</text>
					</view>
					<input password maxlength="18" placeholder="请输入密码" />
				</view>
			</view>
			<view v-show="tabIndex==1">
				<view class="row-input">
					<view class="tip-box">
						<image src="./static/login87_phone.png"></image>
						<text>账号</text>
					</view>
					<input maxlength="11" placeholder="请输入账号" />
				</view>
				<view class="row-input">
					<view class="tip-box">
						<image src="./static/login87_password.png"></image>
						<text>密码</text>
					</view>
					<input maxlength="6" placeholder="请输入密码" />
				</view>
				<view class="row-input"> 
					<view class="tip-box">
						<image src="./static/login87_password.png"></image>
						<text>再次密码</text>
					</view>
					<input maxlength="6" placeholder="请再次输入密码" />
				</view>
			</view>
			<view class="login-btn">
				确定
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabIndex: 0, //tab下标
			}
		},
		methods: {
			// tab点击
			tabClick(id) {
				this.tabIndex = id
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #00b8ff;
	}

	.head {
		height: 30vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;

		.title {
			color: #FFFFFF;
			font-size: 40rpx;
			letter-spacing: 5rpx;
			height: 140rpx;
			font-weight: bold;
		}

		.tab-box {
			display: flex;
			justify-content: center;
			align-items: flex-end;
			height: 80rpx;
			color: #929293;
			font-weight: bold;
			height: 102rpx;
			width: 600rpx;
			border-radius: 20rpx;
			background-color: #FFFFFF;
			box-shadow: 10rpx 20rpx 40rpx #008ab8;

			.row-tab {
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
				font-weight: bold;
				font-size: 35rpx;
				width: 50%;
				height: 100%;

				.tab-active {
					color: #4d88f8;
				}

				.thing {
					margin-top: 10rpx;
					width: 60rpx;
					height: 6rpx;
					background-color: #3884f6;
				}
			}
		}
	}

	.form-box {
		padding: 0rpx 80rpx;
		height: 70vh;
		background-color: #FFFFFF;

		.row-input {
			display: flex;
			flex-direction: column;
			font-size: 30rpx;
			border-bottom: 1rpx solid #cfcfd0;

			.tip-box {
				display: flex;
				justify-content: flex-start;
				align-items: center;
				height: 100rpx;
				font-size: 28rpx;

				image {
					width: 40rpx;
					height: 40rpx;
					margin-right: 10rpx;
				}
			}

			input {
				flex: 1;
				height: 100rpx;
			}
		}

		.login-btn {
			margin-top: 80rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			height: 100rpx;
			letter-spacing: 10rpx;
			background: linear-gradient(to right, #0093ff, #00e7ff);
			color: #FFFFFF;
			border-radius: 10rpx;
			font-size: 30rpx;
		}

	}
</style>