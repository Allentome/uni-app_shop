<template>
	<view class="goods_list">
		<goods-list @goodsItemClick="goGoodsDetail" :goods="goods"></goods-list>
		<view class="isOver" v-if="flag">
			------------底线-------------
		</view>
	</view>
</template>

<script>
	import goodsList from '../../componetns/goods_list/goods_list.vue'
	export default {
		data() {
			return {
				pageindex: 1,
				goods: [],
				flag: false
			}
		},
		components: {
			"goods-list": goodsList
		},
		methods: {
			//获取商品列表的数据
			async getGoodsList(callBack) {
				const res = await this.$myRuquest({
					url: '/api/public/v1/goods/search?pagenum=' + this.pageindex
				})
				// console.log(res)
				this.goods = [...this.goods, ...res.data.message.goods]
				callBack && callBack()
				// console.log(this.goods)
				// uni.stopPullDownRefresh()
			},
			//导航到商品详情页
			goGoodsDetail(id) {
				// console.log(id)
				uni.navigateTo({
					url: '/pages/goods-detail/goods-detail?goods_id=' + id
				})

			}
		},
		onLoad() {
			this.getGoodsList()
		},
		onReachBottom() {
			if (this.goods.length < this.pageindex * 10) return this.flag = true
			// console.log('logbutton')
			this.pageindex++
			this.getGoodsList()
		},
		onPullDownRefresh() {
			console.log('log')
			this.pageindex = 1
			this.goods = []
			this.flag = false
			setTimeout(() => {
				this.getGoodsList(() => {
					uni.stopPullDownRefresh()
				})
			})
		}
	}
</script>

<style lang="scss">
	.goods_list {
		background-color: #eee;
	}

	.isOver {
		width: 100%;
		height: 50px;
		line-height: 50px;
		text-align: center;
		font-size: 28rpx;
	}
</style>
