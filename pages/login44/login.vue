<template>
	<view class="form-box">
		<view class="segmentation">
			<view class="line"></view>
			<view class="node-box">
				<view class="row-node" v-for="i in 5" :key="i">
					<view class="top-round round"></view>
					<view class="bottom-round round"></view>
					<view class="connecting-line"></view>
				</view>
			</view>
		</view>
		<view class="input-box">
			<input class="name" placeholder="Username" maxlength="11" />
			<input placeholder="Password" maxlength="18" password="true" />
		</view>
		<view class="btn-box">Log in</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		methods: {}
	}
</script>

<style lang="scss">
	page {
		background-color: #dfe1e5;
	}

	.form-box {
		position: relative;
		margin: 200rpx auto 0 auto;
		border-radius: 20rpx;
		width: 650rpx;
		background-color: #f8fafb;
		border: 2rpx solid #ccced0;

		.segmentation {
			position: relative;
			width: 100%;
			height: 280rpx;
			border-top-left-radius: 10rpx;
			border-top-right-radius: 10rpx;
			background-color: #f8fafb;

			.line {
				position: absolute;
				top: 50%;
				left: 0;
				width: 100%;
				height: 4rpx;
				background-color: #e0e2e6;
			}

			.node-box {
				position: absolute;
				display: flex;
				justify-content: space-around;
				align-items: center;
				top: 30%;
				width: 100%;
				height: 120rpx;

				.row-node {
					position: relative;
					display: flex;
					flex-direction: column;
					align-items: center;
					width: 50rpx;
					height: 100%;

					.round {
						position: absolute;
						width: 30rpx;
						height: 30rpx;
						border-radius: 100%;
						border: 6rpx solid #c0c2c3;
						background-color: #d8d9da;
					}

					.top-round {
						top: 0;
					}

					.bottom-round {
						bottom: 0;
					}

					.connecting-line {
						position: absolute;
						top: 10%;
						width: 20rpx;
						height: 80%;
						border-radius: 20rpx;
						background-color: #f0f2f3;
						box-shadow:
							0rpx 0rpx 12rpx rgba(0, 0, 0, 0.2)
					}
				}
			}
		}

		.input-box {
			display: flex;
			flex-direction: column;
			border: 4rpx solid #ccced0;
			margin: 0 50rpx;
			border-radius: 10rpx;

			input {
				padding: 0 20rpx;
				height: 108rpx;
				background-color: #e5e7eb;
			}

			.name {
				border-bottom: 4rpx solid #ccced0;
			}
		}

		.btn-box {
			display: flex;
			justify-content: center;
			align-items: center;
			margin: 50rpx;
			height: 120rpx;
			border-radius: 10rpx;
			color: #f0f2f3;
			font-size: 32rpx;
			font-weight: bold;
			background-color: #e0ab55;
		}

	}
</style>