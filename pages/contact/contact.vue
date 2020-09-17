<template>
	<view class="contact">
		<view class="font_back">
			<view class="font">
				关于我们
			</view>
		</view>
		<view class="info">
			<view @click="phone">
				联系电话：400-584-944-555（点击拨打）
			</view>
			<view>
				联系地址：广东省深圳市龙岗区你找不到我
			</view>
		</view>
		<map class="map" :longitude="longitude" :latitude="latitude" :markers="markers"></map>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				longitude: 114.251431,
				latitude: 22.305033,
				markers: [{
					longitude: 114.251431,
					latitude: 22.305033,
					iconPath: '../../static/logo.png',
					width: 30,
					height: 30
				}]
			}
		},
		methods: {
			phone() {
				uni.makePhoneCall({
					phoneNumber: '400-584-944-555'
				})
			}
		}
	}
</script>

<style lang="scss">
	.contact {
		.font_back {
			width: 750rpx;
			height: 320rpx;
			display: block;
			text-align: center;
			background-color: #808080;

			.font {
				color: white;
				font-size: 50px;
				line-height: 150px;
			}
		}

		.info {
			padding: 10rpx 20rpx;
			font-size: 30rpx;

			view {
				line-height: 80rpx;
				border-bottom: 1px solid #eee;
			}
		}

		.map {
			width: 750rpx;
			height: 750rpx;
		}
	}
</style>
