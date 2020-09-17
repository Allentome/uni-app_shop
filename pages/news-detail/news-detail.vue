<template>
	<view class="new_detail">
		<text class="title">{{detail.goods_name}}</text>
		<view class="info">
			<text>发表时间：{{detail.upd_time}}</text>
			<text>浏览：{{detail.goods_price}}</text>
		</view>
		<view class="content">
			{{detail.goods_name}}
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: 0,
				detail: {}
			}
		},
		methods: {
			async getNewDetail() {
				const res = await this.$myRuquest({
					url: '/api/public/v1/goods/detail?goods_id=' + this.id
				})
				// console.log(res)
				this.detail = res.data.message
			}
		},
		onLoad(options) {
			// console.log(options)
			this.id = options.id
			this.getNewDetail()
		}
	}
</script>

<style lang="scss">
	.new_detail {
		font-size: 30rpx;
		padding: 0 20rpx;

		.title {
			text-align: center;
			width: 750rpx;
			display: block;
			margin: 20rpx 0;
		}

		.info {
			display: flex;
			justify-content: space-between;
		}
	}
</style>
