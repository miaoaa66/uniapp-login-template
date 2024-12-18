<template>
	<view>
		<view class="trough">
			<view class="within-trough">
				<view class="show-more" :class="[isShow ? 'show-more-click' : '']">
					<view class="form-box">
						<view class="row">
							<text>账号</text>
							<input placeholder="请输入账号" maxlength="16" />
						</view>
						<view class="row">
							<text>密码</text>
							<input placeholder="请输入密码" password="true" maxlength="18" />
						</view>
						<view class="row">
							<text>再次输入密码</text>
							<input placeholder="请再次输入密码" password="true" maxlength="18" />
						</view>
					</view>
					<view class="btn-box">
						<view class="btn">{{isShow?'注册':'登录'}}</view>
						<view class="tip" @click="showMore">{{isShow?'去登录':'去注册'}}</view>
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
				isShow: false,
			}
		},
		methods: {
			showMore() {
				this.isShow = !this.isShow
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: #666666;
	}

	.trough {
		display: flex;
		justify-content: center;
		align-items: center;
		margin: 200rpx auto 0 auto;
		width: 650rpx;
		height: 140rpx;
		border-radius: 20rpx;
		background-color: #c6c6c6;
		box-shadow: 0rpx 0rpx 12rpx rgba(0, 0, 0, 1);

		.within-trough {
			position: relative;
			width: 90%;
			height: 30rpx;
			background-color: #4f4f4f;
			border-radius: 50rpx;
			box-shadow: 0rpx 0rpx 12rpx rgba(255, 255, 255, 1.0);

			.show-more {
				position: absolute;
				display: flex;
				flex-direction: column;
				top: 15rpx;
				left: 5%;
				width: 90%;
				height: 600rpx;
				border-top: 2rpx solid #a3a3a3;
				box-shadow: 0rpx 0rpx 12rpx rgba(0, 0, 0, 0.6);
				transition: height 1s;
				border-radius: 4rpx;
				background: linear-gradient(to bottom, #c7c6c5 1%, #e9e9e9 10%);

				.form-box {
					display: flex;
					flex-direction: column;
					margin-top: 100rpx;

					.row {
						display: flex;
						flex-direction: column;
						justify-content: center;
						align-items: flex-start;
						padding: 20rpx;

						text {
							height: 40rpx;
							font-size: 28rpx;
						}

						input {
							width: 100%;
							height: 80rpx;
							font-size: 28rpx;
							border-bottom: 2rpx dashed #a3a3a3;
						}
					}
				}

				.btn-box {
					position: absolute;
					display: flex;
					flex-direction: column;
					align-items: center;
					width: 100%;
					bottom: 0rpx;
					font-size: 30rpx;
					background-color: #e9e9e9;
					z-index: 999;

					.btn {
						display: flex;
						justify-content: center;
						align-items: center;
						width: 80%;
						margin: 20rpx 0;
						height: 110rpx;
						border-radius: 4rpx;
						background-color: #979695;
						color: #ece8e7;
					}

					.tip {
						height: 60rpx;
						color: #979695;
					}
				}
			}

			.show-more-click {
				height: 800rpx;
			}
		}
	}
</style>