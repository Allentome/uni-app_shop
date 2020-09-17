<template>
	<view class="cart_page">
		<view class="list" v-for="(item,index) in cart">
			<view :class="item.num>0?'fullbox nodisplay':'fullbox display'">
				<view class="box-left">
					<checkbox-group @change="changeitem(item)">
						<checkbox :checked="item.flag"></checkbox>
					</checkbox-group>
					<!-- <radio value="" :checked="click" @click="cart_click" /> -->
					<image src="http://image2.suning.cn/uimg/b2c/newcatentries/0070078057-000000000634917020_1_400x400.jpg" mode=""></image>
				</view>
				<view class="box-right">
					<view class="font">
						{{item.name}}
					</view>
					<picker @change="bindPickerChange" :value="index" :range="array">
						<view class="uni-input">{{array[index]}}</view>
					</picker>
					<view class="price">
						${{item.price}}
					</view>
					<view class="button">
						<button type="primary" @click="reduce(item)">-</button>
						<text>{{item.num}}</text>
						<button type="primary" @click="puls(item)">+</button>
					</view>
				</view>
			</view>
		</view>

		<view class="pay">
			<view class="pay_left">
				<checkbox-group @change="allchange">
					<checkbox :checked="allchecked"></checkbox>全选
				</checkbox-group>
			</view>
			<view class="pay_right">
				合计：<text>${{allprice}}</text>
				<button type="default">结算</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				array: [
					'中国制造', '美国制造', '巴西制造', '日本制造'
				],
				index: 0,

				addressInfo: {},
				cart: [{
						cartid: 1,
						name: '显示器',
						price: '999',
						num: 1,
						image: '',
						flag: false
					},
					{
						cartid: 2,
						name: '电脑',
						price: '9999',
						num: 1,
						image: '',
						flag: false
					},
				],
				allchecked: false,
			}
		},
		computed: {
			allprice() {
				return this.cart.reduce((prev, cur) => {
					return prev + (cur.flag ? cur.num * cur.price : 0)
				}, 0)

			}
		},
		methods: {
			bindPickerChange: function(e) {
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value
			},

			changeitem(item) {
				item.flag = !item.flag
				if (item.flag) {
					let test = this.cart.every(item => {
						return item.flag === true
					})
					if (test) {
						this.allchecked = true
					} else {
						this.allchecked = false
					}
				} else {
					this.allchecked = false
				}
			},
			allchange() {
				this.allchecked = !this.allchecked
				// 如果 allcheckked 为真  全选被选中时，那么设置每一项都被选中
				if (this.allchecked) {
					this.cart.map(item => {
						item.flag = true
					})
				} else {
					this.cart.map(item => {
						item.flag = false
					})
				}
			},
			puls(item) {
				item.num += 1
			},
			reduce(item) {
				if (item.num == 0) {
					// item.num = 0
					this.cart[1] = {}
				} else {
					item.num -= 1
				}
				// item.num -= 1
			},


		}
	}
</script>

<style lang="scss">
	page {
		background-color: #eee;

	}

	.display {
		display: none;
	}

	.nodisplay {
		display: flex;
	}

	.fullbox {
		margin: 0 auto;
		width: 90%;
		padding: 15rpx;
		border-radius: 25rpx;
		margin-top: 20rpx;
		background-color: white;
		height: 200rpx;
		// display: flex;

		.box-left {
			// background-color: green;
			flex: 1;

			checkbox {
				transform: scale(0.7);
				margin-top: 80rpx;
				float: left;
			}

			image {
				width: 65%;
				height: 100%;
				float: right;
			}
		}

		.box-right {
			// background-color: hotpink;
			padding: 5rpx 10rpx;
			flex: 2;

			.font {
				font-size: 28rpx;
				width: 460rpx;
				white-space: nowrap;
				overflow: hidden;
				text-overflow: ellipsis;
			}

			picker {
				border-radius: 10rpx;
				font-size: 32rpx;
				margin-top: 20rpx;
				background-color: #eee;
			}

			.price {
				margin-top: 20rpx;
				color: red;
			}

			.button {
				text-align: center;
				margin-top: -30rpx;
				margin-right: 50rpx;

				button {
					margin: 0 auto;
					// line-height: 32rpx;
					font-size: 32rpx;
					width: 50rpx;
					height: 50rpx;
					float: right;
					display: flex;
					justify-content: center;
					align-items: center
				}

				text {
					// line-height: 38rpx;
					padding: 0 20rpx;
					float: right;

				}
			}

		}
	}

	.pay {
		margin: 0 auto;
		justify-items: center;
		height: 100rpx;
		width: 100%;
		background-color: white;
		position: absolute;
		bottom: 0;
		// margin-bottom: 0rpx;

		.pay_left {
			float: left;
			font-size: 30rpx;
			margin-left: 20rpx;
			line-height: 100rpx;

			checkbox {
				transform: scale(0.7);
			}
		}

		.pay_right {
			float: right;
			font-size: 30rpx;
			margin-right: 20rpx;
			line-height: 100rpx;

			text {
				color: red;
				margin-right: 20rpx;
			}

			button {
				width: 150rpx;
				color: white;
				border-radius: 35rpx;
				background-color: red;
				margin-top: 10rpx;
				font-size: 30rpx;
				float: right;
				height: 70rpx;
			}
		}

	}
</style>
