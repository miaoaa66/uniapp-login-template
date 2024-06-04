<template>
	<view class="content">
		<view class="tab-box">
			<text @click="tabChange(0)" :class="['name',tabIndex==0?'active':'']">登录</text>
			<text @click="tabChange(1)" :class="['name',tabIndex==1?'active':'']">注册</text>
		</view>
		<view class="form-box">
			<view v-if="tabIndex==0">
				<view class="row-input">
					<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login25/login25_phone.png"></image>
					<input placeholder="用户名/手机号" maxlength="11" />
				</view>
				<view class="row-input">
					<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login25/login25_password.png"></image>
					<input placeholder="密码(6-16位数字字母组合密码)" password maxlength="11" />
				</view>
				<view class="menu-link">
					<text>忘记密码?</text>
				</view>
				<view class="login-btn">登录</view>
			</view>

			<view v-else>
				<view class="row-input">
					<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login25/login25_phone.png"></image>
					<input placeholder="用户名/手机号" maxlength="11" />
				</view>
				<view class="row-input">
					<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login25/login25_password.png"></image>
					<input placeholder="密码(6-16位数字字母组合密码)" password maxlength="11" />
				</view>
				<view class="row-input">
					<image src="https://mp-dc95b988-31eb-4147-82e1-c097f0893940.cdn.bspapp.com/登录模板库/login25/login25_code.png"></image>
					<input placeholder="输入验证码" maxlength="6" />
					<view class="code">
						<text>获取验证码</text>
					</view>
				</view>
				<view class="login-btn" style="margin: 120rpx 0rpx;">注册</view>
				<view class="tip-link">
					注册账号代表同意<text>《用户协议》</text>
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
			// tab切换
			tabChange(id) {
				this.tabIndex = id
			}
		}
	}
</script>

<style lang="scss">
	.tab-box {
		padding-left: 40rpx;
		height: 200rpx;
		display: flex;
		justify-content: flex-start;
		align-items: center;

		text {
			text-align: center;
			width: 160rpx;
			font-size: 36rpx;
			color: #4b535c;
		}

		.active {
			padding-bottom: 20rpx;
			font-size: 60rpx;
			color: #505860;
			font-weight: bold;
		}
	}

	.form-box {
		padding: 40rpx 60rpx;

		.row-input {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			padding-top: 80rpx;
			height: 80rpx;
			font-size: 30rpx;
			border-bottom: 1rpx solid #f1f1f1;

			image {
				margin: 0 30rpx;
				flex-shrink: 0;
				width: 30rpx;
				height: 40rpx;
			}

			input {
				padding-left: 20rpx;
				font-size: 30rpx;
				flex: 1;
			}

			.code {
				display: flex;
				justify-content: center;
				align-items: center;
				width: 160rpx;
				font-size: 28rpx;
				color: #fdb216;
			}
		}

		.menu-link {
			display: flex;
			justify-content: flex-end;
			line-height: 100rpx;
			font-size: 28rpx;
			color: #fdb216;
			letter-spacing: 3rpx;
		}

		.login-btn {
			margin-top: 40rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			border-radius: 10rpx;
			width: 100%;
			height: 100rpx;
			font-size: 40rpx;
			color: #FFFFFF;
			background-color: #fdb216;
			box-shadow: 0rpx 5rpx 30rpx #fdb216;
		}

		.tip-link {
			display: flex;
			justify-content: center;
			font-size: 28rpx;
			color: #d3d3d3;

			text {
				color: #fdb216;
			}
		}
	}
</style>
