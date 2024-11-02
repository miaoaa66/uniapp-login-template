<template>
	<view class="content">
		<view class="ad">
			<ad unit-id="adunit-5e3096ae624a7eca"></ad>
		</view>
		<view class="row-box" v-for="(item,index) in listData" :key="index" @click="goto(index)">
			<view class="cover">
				<image mode="aspectFit" :src="item.cover"></image>
			</view>
			<view class="tag-box">
				<view class="title">Login{{index+1}}</view>
				<view class="tag">
					<text v-for="(item2,index2) in item.tags" :key="index2">{{item2}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import menu from "../common/data.js"
	export default {
		data() {
			return {
				listData: menu.data
			}
		},
		onLoad() {
		},
		onReady() {
		},
		methods: {
			//goto
			goto(index) {
				uni.navigateTo({
					url: this.listData[index].path,
					fail(err) {
						console.log(err);
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #859926;
	}

	.content {
		padding: 20rpx;
		display: flex;
		flex-wrap: wrap;

		.ad {
			width: 100%;
			margin: 0rpx 20rpx;
		}

		.row-box {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			border-radius: 10rpx;
			margin: 15rpx;
			padding: 0 15rpx;
			width: 327rpx;
			height: 160rpx;
			background-color: #fef9d1;
			border: 5rpx solid #8a7719;
			box-shadow: 0rpx 8rpx 10rpx #54690d;
			box-sizing: border-box;

			.cover {
				width: 30%;
				display: flex;
				justify-content: center;

				image {
					width: 70rpx;
					height: 112rpx;
					border-radius: 5rpx;
					border: 3rpx solid #3c7854;
					box-sizing: border-box;
				}
			}

			.tag-box {
				padding: 10rpx;
				width: 70%;

				.title {
					display: flex;
					justify-content: center;
					align-items: center;
					height: 45rpx;
					margin-bottom: 10rpx;
					letter-spacing: 7rpx;
					-webkit-text-fill-color: #ffe0b9;
					-webkit-text-stroke: 2rpx #000;
					font-size: 30rpx;
					font-weight: bold;
					background-color: #859926;
					border: 5rpx solid #69781d;
					border-radius: 5rpx;
					box-sizing: border-box;
				}

				.tag {
					display: flex;
					flex-wrap: wrap;
					font-size: 18rpx;

					text {
						margin: 5rpx;
						padding: 2rpx 6rpx;
						border-radius: 5rpx;
						color: #FFFFFF;
					}
				}

				.tag text:nth-child(1n+0) {
					background-color: #57b183
				}

				.tag text:nth-child(2n+0) {
					background-color: #eaab2d
				}

				.tag text:nth-child(3n+0) {
					background-color: #87330d
				}

				.tag text:nth-child(4n+0) {
					background-color: #4a71ca
				}

				.tag text:nth-child(5n+0) {
					background-color: #a67d8a
				}

				.tag text:nth-child(6n+0) {
					background-color: #519ddf
				}
			}
		}
	}
</style>
