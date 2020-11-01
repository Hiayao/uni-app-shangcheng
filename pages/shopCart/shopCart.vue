<template>
	<view class="All">
		<!-- 发货提醒 -->
		<view class="notes flex jusca">
			<view class="flex alii-center" v-for="item in notes" :key="item.id">
				<image src="../../image/yuan.png" mode="" class="yuan"></image>
				<view class="">
					{{item}}
				</view>
			</view>
		</view>

		<!-- <u-checkbox-group @change="checkboxGroupChange">
			<u-checkbox @change="checkboxChange(item)" shape="circle"  v-model="item.checked"
			 :name="item.goods_name">
			 <view class="suibian"> -->
		<u-swipe-action :show="item.show" :index="index" v-for="(item, index) in cartList" :key="item.id" @click="del(item)"
		 @open="open" :options="options">

			<view class="item u-border-bottom alii-center">
				<checkbox style="transform:scale(0.7)" value="" :checked="item.checked" @click="checkOne(item,index)" />
				<view class="f4 flex">
					<image mode="aspectFill" :src="item.list_pic_url" class="image" />
					<!-- 此层wrap在此为必写的，否则可能会出现标题定位错误 -->
					<view class="title-wrap">
						<view class="title u-line-2">{{ item.goods_name }}</view>
						<view class="">
							￥{{item.retail_price}}
						</view>
					</view>
				</view>
				<view class="f1">
					X {{item.number}}
				</view>
			</view>
		</u-swipe-action>
		<!-- </view>
			</u-checkbox>
		</u-checkbox-group> -->
		
		<!-- 删除弹出层 -->
		<u-modal v-model="shows" :content="content" :show-cancel-button='true' @confirm='sureDel'></u-modal>
		<view class="buyAll flex alii-center juscb">
			<view class="buyOne flex alii-center">
				<checkbox style="transform:scale(0.7)" @click="checkAll" :checked="checkdAll" />
				<view class="">
					全选({{num2}})
				</view>
			</view>
			<view class="buyOne flex alii-center">
				<view class="">
					￥{{total}}
				</view>
				<view class="buy flex alii-center jus-center fw6" @click="gotoBuy">
					下单
				</view>
			</view>

		</view>
	</view>
</template>

<script>
	export default {
		name: "",
		components: {

		},
		props: {},
		data() {
			return {
				notes: ["30天无忧退货", "48小时快速退款", "满88元免邮费"],

				disabled: false, //是否禁止某个swipeAction滑动
				btnWidth: 180, //按钮宽度，单位rpx
				show: false, //打开或者关闭某个组件
				options: [{
					text: '删除',
					style: {
						backgroundColor: '#dd524d'
					}
				}],
				cartList: [], //购物车数据
				// 删除确认弹窗
				shows: false,
				content: '是否删除所选商品',

				id: '', //商品id
				checked: false,
				checkdAll: false,
				arr: [], //用来接收选中的商品
				totals:0,  //已选商品总价
			}
		},
		methods: {
			// 点击删除弹出确认弹窗
			del(item) {
				this.shows = true
				this.id = item.id
			},
			// 确认删除商品
			sureDel() {
				uni.request({
					// #ifdef H5
					url: `/api/cart/deleteAction?id=${this.id}`,
					// #endif
					// #ifdef MP-WEIXIN || APP-PLUS
					url: `${this.$api}/cart/deleteAction?id=${this.id}`,
					// #endif
					method: 'GET',
					data: {},
					success: res => {
						console.log(res);
						this.getCart()
						this.checkdAll = false
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 如果打开一个的时候，不需要关闭其他，则无需实现本方法
			// 暂时无法生效
			open(index) {
				// 先将正在被操作的swipeAction标记为打开状态，否则由于props的特性限制，
				// 原本为'false'，再次设置为'false'会无效
				this.cartList.map(item => {
					item.show = false
				})
				// console.log(this.cartList);
				this.cartList[index].show = true;
				this.cartList.map((val, idx) => {
					// console.log(val,idx);
					if (index !== idx) {
						this.cartList[idx].show = false;
						// console.log(idx);
					}
				})
			},
			// 查询购物车数据
			getCart() {
				uni.showLoading({
					title:'加载中'
				})
				uni.request({
					// #ifdef H5
					url: `/api/cart/cartList?openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
					// #endif
					// #ifdef MP-WEIXIN || APP-PLUS 
					url: `${this.$api}/cart/cartList?openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
					// #endif
					method: 'GET',
					data: {},
					success: res => {
						// console.log(res);
						uni.hideLoading()//取消加载
						res.data.data.map(item => {
							item.checked = false
						})
						this.cartList = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 选择单项
			checkOne(item, index) {
				console.log(item, index);
				item.checked = !item.checked
				this.checkdAll = this.cartList.every(item => {
					return item.checked === true
				})
			},
			// 全选
			checkAll() {
				console.log(this.checkdAll);
				this.checkdAll = !this.checkdAll
				this.cartList.map(item => {
					if (this.checkdAll === true) {
						item.checked = true
						// this.arr.push(item)
					}
					if (this.checkdAll === false) {
						item.checked = false
						// this.arr = []
					}
				})
			},
			gotoBuy(){
				uni.navigateTo({
					url:`../buyGoods/buyGoods?total=${this.totals}&arr=${this.arr}`
				})
			},
		},
		mounted() {

		},
		onLoad() {

		},
		onShow() {
			this.getCart()
			this.checkdAll = false
		},
		filters: {

		},
		computed: {
			// 总价
			total() {
				let num = 0;
				this.cartList.map(item => {
					 if (item.checked === true) {
						num += item.number * item.retail_price;
						this.totals = num
						}
				});
				return num;
			},
			// 选择的商品数量
			num2() {
			      let num2 = 0;
			      let lists = []
			      this.cartList.map(item => {
			        if (item.checked === true) {
			          lists.push(item);
			          num2 = lists.length;
					  this.arr = JSON.stringify(lists)
			        }
			      });
				   // console.log(this.arr);
			      return num2;
			    },
		},
		watch: {

		},
		directives: {

		}
	}
</script>

<style scoped lang="scss">
	page {
		background: rgb(244, 244, 244);
	}

	.notes {
		background: rgb(238, 238, 238);
		font-size: 24rpx;
		height: 70rpx;
	}

	.yuan {
		width: 10rpx;
		height: 10rpx;
		margin-right: 10rpx;
	}

	.item {
		display: flex;
		padding: 20rpx;
		justify-content: space-between;
	}

	.image {
		width: 120rpx;
		flex: 0 0 120rpx;
		height: 120rpx;
		margin-right: 20rpx;
		border-radius: 12rpx;
	}

	.title {
		text-align: left;
		font-size: 28rpx;
		color: $u-content-color;
		margin-top: 20rpx;
	}

	.buy {
		height: 100%;
		background: rgb(180, 40, 45);
		width: 160rpx;
		color: #fff;
		margin-left: 20rpx;
	}

	.buyAll {
		height: 100rpx;
		padding-left: 20rpx;
		position: fixed;
		bottom: 99rpx;
		width: 100%;
		border-top: 1rpx solid #d3d3d3;
		background: #fff;
	}

	.buyOne {
		height: 100%;
	}
</style>
