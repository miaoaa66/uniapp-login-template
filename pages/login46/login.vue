<template>
	<view class="content">
		<view class="title">xx后台管理系统</view>
		<view class="form-box">
			<view class="left-pendant pendant"></view>
			<view class="right-pendant pendant"></view>
			<view class="tip">Welcome</view>
			<view class="input-box">
				<input placeholder="用户名" maxlength="11" />
			</view>
			<view class="input-box">
				<input placeholder="密码" maxlength="18" password="true" />
			</view>
			<view class="btn-box">
				<view class="remember">
					<checkbox class="remember" value="" color="#0055ff" style="transform:scale(0.7)" />忘记密码
				</view>
				<view class="btn">登录</view>
			</view>
		</view>
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
		background-color: #414246;
	}

	.content {
		display: flex;
		flex-direction: column;
	}

	.title {
		display: flex;
		justify-content: center;
		align-items: center;
		letter-spacing: 8rpx;
		font-size: 60rpx;
		height: 30vh;
		color: #f8fafb;
	}

	.form-box {
		position: relative;
		display: flex;
		flex-direction: column;
		margin: 0 auto;
		border-radius: 10rpx;
		width: 650rpx;
		background-color: #f8fafb;
		border: 2rpx solid #ccced0;

		.pendant {
			position: absolute;
			width: 20rpx;
			height: 45rpx;
			background-color: #dedddd;
			border-radius: 2rpx;
			box-shadow: rgba(77, 77, 77, 0.5) 0rpx 4rpx 6rpx 4rpx;
		}

		.left-pendant {
			top: -20rpx;
			left: 100rpx;
		}

		.right-pendant {
			top: -20rpx;
			right: 100rpx;
		}

		.tip {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 100rpx;
			margin-bottom: 20rpx;
			font-size: 28rpx;
			color: #a8aaab;
			border-bottom: 2rpx dashed #ccced0;
		}

		.input-box {
			display: flex;
			flex-direction: column;
			border: 2rpx solid #ccced0;
			margin: 20rpx 50rpx;
			border-radius: 10rpx;

			input {
				padding: 0 20rpx;
				height: 80rpx;
				background-color: #fbfbfb;
			}
		}

		.btn-box {
			display: flex;
			justify-content: space-between;
			align-items: center;
			margin: 50rpx;
			height: 80rpx;

			.remember {
				display: flex;
				justify-content: center;
				align-items: center;
				font-size: 28rpx;
				color: #a8aaab;
			}

			.btn {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 140rpx;
				height: 60rpx;
				font-size: 28rpx;
				color: #f8fafb;
				background-color: #4c89e3;
				border: 10rpx solid #e5e5e6;
				border-radius: 10rpx;
			}
		}

	}
</style>