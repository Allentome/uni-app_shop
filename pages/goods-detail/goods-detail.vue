<template>
	<view class="goods_detail">
		<swiper indicator-dots>
			<swiper-item v-for="(item,index) in swipers.pics">
				<image :src="item.pics_big"></image>
			</swiper-item>
		</swiper>
		<view class="box1">
			<view class="price">
				<text>${{swipers.goods_price}}</text>
				<text>${{swipers.goods_price}}</text>
			</view>
			<view class="goods_name">
				{{swipers.goods_name}}
			</view>
		</view>
		<view class="line"></view>
		<view class="box2">
			<view>货号：{{swipers.goods_id}}</view>
			<view>库存：{{swipers.goods_number}}</view>
		</view>
		<view class="line"></view>
		<view class="box3">
			<view class="tit">详情</view>
			<!-- <view class="content">内容</view> -->
		</view>
		<rich-text :nodes="swipers.goods_introduce"></rich-text>
		<view class="goods_nav">
			<uni-goods-nav :fill="true" :options="options" :buttonGroup="buttonGroup" @click="onClick" @buttonClick="buttonClick" />
		</view>
	</view>
</template>

<script>
	import uniGoodsNav from '@/components/uni-goods-nav/uni-goods-nav.vue'
	export default {
		data() {
			return {
				id: 0,
				swipers: {},
				options: [{
					icon: 'headphones',
					text: '客服'
				}, {
					icon: 'shop',
					text: '店铺',
				}, {
					icon: 'cart',
					text: '购物车',
					info: 2
				}],
				buttonGroup: [{
						text: '加入购物车',
						backgroundColor: '#ff0000',
						color: '#fff'
					},
					{
						text: '立即购买',
						backgroundColor: '#ffa200',
						color: '#fff'
					}
				]
			}
		},
		methods: {
			async getSwiper() {
				const res = await this.$myRuquest({
					url: '/api/public/v1/goods/detail?goods_id=' + this.id
				})
				// console.log(res.data.message)
				this.swipers = res.data.message
				console.log(this.swipers)
			},
			onClick(e) {
				uni.showToast({
					title: `点击${e.content.text}`,
					icon: 'none'
				})
			},
			buttonClick(e) {
				console.log(e)
				this.options[2].info++
			}
		},
		onLoad(options) {
			// console.log(options)
			this.id = options.goods_id
			this.getSwiper()
		},
		components: {
			uniGoodsNav
		}
	}
</script>

<style lang="scss">
	.line {
		font-size: 15rpx;
		height: 10rpx;
		width: 750rpx;
		background: #eee;
		color: #eee;
	}
	.goods_nav{
		position: fixed;
		bottom: 0;
		width: 100%;
	}

	.goods_detail {
		swiper {
			height: 700rpx;
		}

		.box1 {
			padding: 10px;

			.price {
				font-size: 35rpx;
				color: $shop-color;
				line-height: 80rpx;

				text:nth-child(2) {
					color: #ccc;
					font-size: 28rpx;
					text-decoration: line-through;
					margin-left: 20rpx;
				}
			}

			.goods_name {
				font-size: 32rpx;
				line-height: 60rpx;
			}
		}

		.box2 {
			padding: 0 10px;
			font-size: 32rpx;
			line-height: 70rpx;
		}

		.box3 {
			.tit {
				font-size: 32rpx;
				padding-left: 10px;
				border-bottom: 1px solid #eee;
				line-height: 70rpx;
			}

			.content {
				padding: 10px;
				font-size: 28rpx;
				color: #333;
			}
		}

		image {
			width: 100%;
			height: 100%;
		}

	}
</style>
