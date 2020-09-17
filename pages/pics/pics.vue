<template>
	<view class="pics">
		<scroll-view class="left" scroll-y>
			<view @click="leftClickHandle(index,item.cat_id)" :class="active===index?'active':''" v-for="(item,index) in cates"
			 :key="item.cat_id">{{item.cat_name}}</view>
		</scroll-view>
		<scroll-view class="right" scroll-y>
			<view class="item" v-for="item in seconDate.children" :key="item.cat_id">
				<image @click="previewImg(item.cat_icon)" :src="item.cat_icon"></image>
				<text>{{item.cat_name}}</text>
			</view>
			<text v-if="seconDate.length === 0">暂无数据</text>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cates: [],
				active: 0,
				seconDate: []
			}
		},
		methods: {
			async getPicsate() {
				const res = await this.$myRuquest({
					url: '/api/public/v1/categories'
				})
				// console.log(res)
				this.cates = res.data.message
				this.leftClickHandle(0, this.cates[0].cat_id)
			},
			async leftClickHandle(index, id) {
				// console.log(index)
				this.active = index
				const res = await this.$myRuquest({
					url: '/api/public/v1/categories'
				})
				this.seconDate = res.data.message[index].children[0]
				// console.log(this.seconDate)
			},
			previewImg(current) {
				const urls = this.seconDate.children.map(item => {
					return item.cat_icon
				})
				// console.log(urls)
				uni.previewImage({
					current,
					urls
				})
			}
		},
		onLoad() {
			this.getPicsate()
		}
	}
</script>

<style lang="scss">
	page {
		height: 100%;
	}

	.pics {
		height: 100%;
		display: flex;

		.left {
			width: 200rpx;
			height: 100%;
			border-right: 1px solid #eee;

			view {
				height: 60px;
				line-height: 60px;
				color: #333;
				text-align: center;
				font-size: 30rpx;
				border-top: 1px solid #eee;
			}

			.active {
				background-color: $shop-color;
				color: #fff;
			}
		}

		.right {
			height: 100%;
			width: 530rpx;
			margin: 0 auto;

			.item {
				image {
					width: 530rpx;
					height: 530rpx;
					border-radius: 5px;
				}

				text {
					font-size: 30rpx;
					line-height: 60rpx;
				}
			}
		}
	}

	// .pics {
	// 	height: 100%;
	// 	display: flex;
	// }
</style>
