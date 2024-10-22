<template>
	<view class="content">
		<view class="panda-box">
			<!-- 头部分 -->
			<view class="head-box">
				<view class="head"></view>
				<!-- 左耳 -->
				<view class="left-ear ear"></view>
				<!-- 右耳 -->
				<view class="right-ear ear"></view>
				<!-- 左眼 -->
				<view class="left-eye eye">
					<view class="eyeball">
						<view class="ball"></view>
					</view>
				</view>
				<!-- 右眼 -->
				<view class="right-eye eye">
					<view class="eyeball">
						<view class="ball"></view>
					</view>
				</view>
				<!-- 鼻子 -->
				<view class="nose"></view>
			</view>
			<!-- 肚子部分 -->
			<view class="abdomen">
				<!-- 左脚 -->
				<view class="left-foot foot">
					<!-- 脚指部分 -->
					<view class="toe-box">
						<view class="toe"></view>
						<view class="toe"></view>
						<view class="toe"></view>
					</view>
					<!-- 脚板 -->
					<view class="board"></view>
				</view>

				<!-- 右脚 -->
				<view class="right-foot foot">
					<!-- 脚指部分 -->
					<view class="toe-box">
						<view class="toe"></view>
						<view class="toe"></view>
						<view class="toe"></view>
					</view>
					<!-- 脚板 -->
					<view class="board"></view>
				</view>

				<!-- 登录部分 -->
				<view class="login-box" :animation="animationData">
					<view class="title">用户登录</view>
					<view class="row">
						<text>用户名</text>
						<input maxlength="18" />
					</view>
					<view class="row">
						<text>密码</text>
						<input maxlength="18" @focus="look" @blur="notLook" password="true" />
					</view>
					<view class="login-btn">登录</view>

					<!-- 手部分 -->
					<view class="left-hand hand-box">
						<view class="hand"></view>
						<view class="hand"></view>
						<view class="hand"></view>
					</view>
					<!-- 手部分 -->
					<view class="right-hand  hand-box">
						<view class="hand"></view>
						<view class="hand"></view>
						<view class="hand"></view>
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
				animationData: {},
			}
		},
		onLoad(options) {
			this.animation = uni.createAnimation()
		},
		onUnload() {
			this.animationData = {}
		},
		methods: {

			// 观看
			look() {
				this.animation.translateY(-45).step({
					duration: 500
				})
				this.animationData = this.animation.export()
			},

			// 不观看
			notLook() {
				this.animation.translateY(0).step({
					duration: 500
				})
				this.animationData = this.animation.export()
			}
		}
	}
</script>

<style lang="scss">
	.content {
		width: 100vw;
		height: 100vh;
		display: flex;
		justify-content: center;
		align-items: flex-start;
		background-color: #29c3ab;

		.head-box {
			margin: 50% auto 0 auto;
			position: relative;
			display: flex;
			justify-content: center;
			align-items: flex-end;
			width: 300rpx;
			height: 300rpx;

			.head {
				position: absolute;
				width: 100%;
				height: 90%;
				border-radius: 100%;
				background-color: #fff;
				z-index: 2;
			}

			.ear {
				width: 100rpx;
				height: 100rpx;
				background-color: #000000;
				border-radius: 100%;
				z-index: 1;
			}

			.left-ear {
				position: absolute;
				top: 4rpx;
				left: 10rpx;
				z-index: 1;
			}

			.right-ear {
				position: absolute;
				top: 4rpx;
				right: 10rpx;
				z-index: 1;
			}

			.nose {
				position: absolute;
				top: 60%;
				left: 40%;
				width: 20%;
				height: 10%;
				background-color: #000000;
				border-radius: 40%;
				z-index: 3;
			}

			.eye {
				position: relative;
				width: 120rpx;
				height: 70rpx;
				background-color: #000000;
				border-radius: 100%;
				z-index: 2;
			}

			.left-eye {
				position: absolute;
				top: 100rpx;
				left: 16rpx;
				transform: rotate(145deg);
			}

			.right-eye {
				position: absolute;
				top: 100rpx;
				right: 16rpx;
				transform: rotate(35deg);
			}

			.eyeball {
				position: absolute;
				bottom: 14rpx;
				right: 40rpx;
				width: 40rpx;
				height: 40rpx;
				background-color: #fff;
				border-radius: 100%;

				.ball {
					position: absolute;
					top: 6rpx;
					width: 30rpx;
					height: 30rpx;
					border-radius: 100%;
					background-color: #000000;
				}
			}
		}

		.abdomen {
			position: relative;
			width: 400rpx;
			height: 400rpx;
			background-color: #fff;
			border-radius: 45%;
			box-shadow: rgba(0, 0, 0, 0.4) 0rpx 2rpx 8rpx 0rpx;

			.foot {
				position: absolute;
				display: flex;
				flex-direction: column;
				align-items: center;
				top: 250rpx;
				width: 110rpx;
				height: 150rpx;
				background-color: #000000;
				border-top-left-radius: 80rpx;
				border-top-right-radius: 80rpx;
				border-bottom-left-radius: 100%;
				border-bottom-right-radius: 100%;
				z-index: 9;

				.toe-box {
					display: flex;
					justify-content: space-around;
					align-items: center;
					width: 100rpx;
					height: 70rpx;

					.toe {
						width: 22rpx;
						height: 40rpx;
						border-radius: 50%;
						background-color: #434343;
					}
				}

				.board {
					width: 60rpx;
					height: 70rpx;
					border-radius: 50%;
					background-color: #434343;
				}
			}

			.left-foot {
				left: -50rpx;
			}

			.right-foot {
				right: -50rpx;
			}

			.login-box {
				position: absolute;
				display: flex;
				flex-direction: column;
				left: -75rpx;
				top: -100rpx;
				width: 550rpx;
				height: 450rpx;
				background-color: #fff;
				z-index: 6;
				border-radius: 8rpx;
				box-shadow: rgba(0, 0, 0, 0.4) 0rpx 2rpx 8rpx 0rpx;

				.title {
					display: flex;
					justify-content: center;
					align-items: flex-end;
					width: 100%;
					height: 100rpx;
					font-size: 40rpx;
					font-weight: bold;
					color: #29c3ab;
				}

				.row {
					margin: 20rpx 40rpx;
					display: flex;
					flex-direction: column;
					border-bottom: 3rpx solid #29c3ab;

					text {
						font-size: 24rpx;
						color: #29c3ab;
						font-weight: bold;
					}

					input {
						font-size: 28rpx;
					}

				}

				.login-btn {
					margin: 10rpx auto;
					display: flex;
					justify-content: center;
					align-items: center;
					width: 200rpx;
					height: 60rpx;
					font-size: 28rpx;
					border-radius: 10rpx;
					background-color: #29c3ab;
					color: #fff;
					letter-spacing: 10rpx;
				}

				.hand-box {
					position: absolute;
					display: flex;
					flex-direction: column;
					top: 100rpx;


					.hand {
						width: 40rpx;
						height: 25rpx;
						border-radius: 20rpx;
						background-color: #000000;
					}
				}

				.left-hand {
					left: -20rpx;
				}

				.right-hand {
					right: -20rpx;
				}
			}

		}
	}
</style>