<template>
	<view class="content">
		<view class="level">
			<view class="level2">
				<view class="form-box">
					<view class="tab-box">
						<view class="cell-tab">
							<text class="name" :class="tabIndex==0?'active':''" @click="tabChange(0)">登录</text>
							<text v-if="tabIndex==0" class="thing"></text>
						</view>
						<view class="cell-tab">
							<text class="name" :class="tabIndex==1?'active':''" @click="tabChange(1)">注册</text>
							<text v-if="tabIndex==1" class="thing"></text>
						</view>
					</view>
					<view v-if="tabIndex==0">
						<view class="row-input">
							<input placeholder="用户名" maxlength="12" />
						</view>
						<view class="row-input">
							<input placeholder="密码" password maxlength="18" />
						</view>
						<view class="login-btn">立即登录</view>
						<view class="link-tip">
							没有账号?
							<text @click="tabChange(1)">立即注册</text>
						</view>
					</view>
					<view v-else>
						<view class="row-input">
							<input placeholder="手机号" maxlength="11" />
						</view>
						<view class="row-input">
							<input placeholder="验证码" password maxlength="6" />
							<view class="code">
								<text>验证码</text>
							</view>
						</view>
						<view class="login-btn">立即注册</view>
						<view class="link-tip">
							已有账号?
							<text @click="tabChange(0)">立即登录</text>
						</view>
					</view>
				</view>
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

<style lang="scss" scoped>
	page {
		background-color: #212a33;
	}

	.level {
		position: fixed;
		top: 15vh;
		left: 0;
		bottom: 15vh;
		width: 650rpx;
		height: 70vh;
		border-top-right-radius: 50rpx;
		border-bottom-right-radius: 50rpx;
		background-color: #e2ba97;

		.level2 {
			display: flex;
			justify-content: flex-end;
			margin-top: 7%;
			width: 600rpx;
			height: 90%;
			border-top-right-radius: 50rpx;
			border-bottom-right-radius: 50rpx;
			background-color: #FFFFFF;
			box-shadow: 7rpx 2rpx 15rpx #c89575;

			.form-box {
				padding: 0rpx 60rpx;
				width: 500rpx;
				height: 100%;
				background-color: #FFFFFF;
				border-top-right-radius: 50rpx;
				border-bottom-right-radius: 50rpx;
				box-shadow: -10rpx 0rpx 50rpx #e6e6e6;

				.tab-box {
					display: flex;
					justify-content: center;
					align-items: center;
					height: 200rpx;

					.cell-tab {
						display: flex;
						justify-content: flex-start;
						align-items: center;
						flex-direction: column;
						width: 180rpx;
						margin-top: 120rpx;
						height: 100%;

						.name {
							font-size: 40rpx;
							font-weight: bold;
							color: #c6c6c6;
						}

						.active {
							color: #000000;
						}

						.thing {
							margin-top: 10rpx;
							width: 80rpx;
							height: 12rpx;
							border-radius: 100rpx;
							background-color: #000000;
						}
					}
				}

				.row-input {
					padding: 0 40rpx;
					display: flex;
					align-items: center;
					height: 100rpx;
					border: 1rpx solid #ebebeb;
					border-radius: 50rpx;
					margin-bottom: 50rpx;

					input {
						width: 100%;
					}

					.code {
						margin-left: 20rpx;
						width: 120rpx;
						color: #000000;
						font-size: 28rpx;
					}
				}

				.login-btn {
					display: flex;
					align-items: center;
					justify-content: center;
					height: 100rpx;
					color: #e3e3e3;
					border-radius: 50rpx;
					letter-spacing: 10rpx;
					font-size: 35rpx;
					font-weight: bold;
					background-color: #000000;
				}

				.link-tip {
					display: flex;
					justify-content: center;
					padding-top: 60rpx;
					font-size: 28rpx;
					font-weight: bold;
					color: #bbbbbb;

					text {
						padding-left: 20rpx;
						color: #000000;
					}
				}
			}
		}
	}
</style>
