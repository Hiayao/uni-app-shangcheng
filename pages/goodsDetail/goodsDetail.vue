<template>
	<view>
		<!-- 轮播图 -->
		<u-swiper :list="gallery" name="img_url" :height="700" :border-radius="0" img-mode="heightFix"></u-swiper>
		<!-- 发货提醒 -->
		<view class="notes flex jusca">
			<view class="flex alii-center" v-for="item in notes" :key="item.id">
				<image src="../../image/yuan.png" mode="" class="yuan"></image>
				<view class="">
					{{item}}
				</view>
			</view>
		</view>
		<!-- 商品信息 -->
		<view class="info tac">
			<view class="name">
				{{info.name}}
			</view>
			<view class="">
				{{info.goods_brief}}
			</view>
			<view class="price">
				￥{{info.retail_price}}
			</view>
		</view>
		<!-- 商品参数 -->
		<view class="attributeAll">
			<view class="attributeTitle">
				商品参数
			</view>
			<view class="">
				<view class="attributeOne flex" v-for="item in attribute" :key="item.id">
					<view class="attributeName">
						{{item.name}}
					</view>
					<view class="">
						{{item.value}}
					</view>
				</view>
			</view>
		</view>
		<!-- 商品详情 -->
		<view class="desc">
			<u-parse :html="info.goods_desc"></u-parse>
		</view>
		<!-- 常见问题 -->
		<view class="">
			<view class="tac">
				—— 常见问题 ——
			</view>
			<view class="issueAll">
				<view class="" v-for="item in issue" :key="item.id">
					<view class="flex alii-center">
						<image src="../../image/dian.png" mode="" class="dian"></image>
						<view class="">
							{{item.question}}
						</view>
					</view>
					<view class="answer">
						{{item.answer}}
					</view>
				</view>
			</view>
		</view>
		<!-- 大家都在看 -->
		<view class="">
			<view class="tac">
				—— 大家都在看 ——
			</view>
			<view class="flex fwrap productListAll">
				<view class="productList" v-for="(item,index) in productList" :key="item.id" :class="index%2 == 0?'border':''" @click="gotoDetail(item)">
					<image :src="item.list_pic_url" mode="" class="list_pic_url"></image>
					<view class="tac thidden-one">
						{{item.name}}
					</view>
					<view class="retail_price tac">
						￥{{item.retail_price}}
					</view>
				</view>
			</view>
		</view>
		<!-- 商品导航 -->
		<uni-goods-nav :fill="true" :options="options" :buttonGroup="buttonGroup" @click="onClick" @buttonClick="buttonClick"
		 class="nav" />
		<!-- 点击加入购物车弹出层 -->
		<u-popup v-model="show" mode="bottom" border-radius="14" :closeable="true">
			<view class="flex alii-center">
				<image :src="info.list_pic_url" mode="" class="pic_url"></image>
				<view class="">
					<view class="">
						{{info.name}}
					</view>
					<view class="priceBottom">
						价格 ￥{{info.retail_price}}
					</view>
					<view style="color: #999;">
						请选择数量
					</view>
				</view>
			</view>

			<view class="num flex alii-center">
				<view class="numName">
					数量
				</view>
				<u-number-box v-model="value" :min="1" @change="valChange"></u-number-box>
				<u-button type="success" size="mini" @click='add'>加入购物车</u-button>
			</view>

		</u-popup>
		<!-- 点击立即购买弹出层 -->
		<u-popup v-model="shows" mode="bottom" border-radius="14" :closeable="true">
			<view class="flex alii-center">
				<image :src="info.list_pic_url" mode="" class="pic_url"></image>
				<view class="">
					<view class="">
						{{info.name}}
					</view>
					<view class="priceBottom">
						价格 ￥{{info.retail_price}}
					</view>
					<view style="color: #999;">
						请选择数量
					</view>
				</view>
			</view>

			<view class="num flex alii-center">
				<view class="numName">
					数量
				</view>
				<u-number-box v-model="value" :min="1" @change="valChange"></u-number-box>
				<u-button type="success" size="mini" @click='buy'>立即购买</u-button>
			</view>

		</u-popup>
	</view>
</template>

<script>
	import uniGoodsNav from '@/components/uni-goods-nav/uni-goods-nav.vue'
	export default {
		name: "",
		components: {
			uniGoodsNav
		},
		props: {},
		data() {
			return {
				info: {}, //商品信息
				gallery: [], //商品轮播图
				notes: ["30天无忧退货", "48小时快速退款", "满88元免邮费"],
				attribute: [], //商品参数
				issue: [], //常见问题
				productList: [], //大家都在看
				id: '', //商品id
				show: false, //控制加购弹出层
				shows: false, //控制立即购买弹出层
				value: 1, //步进器数量
				options: [{
						icon: 'heart',
						text: '收藏',
					},
					{
						icon: 'cart',
						text: '购物车',
						info: 0
					}
				],
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
			onClick(e) {
				// console.log(this.id);
				if (e.index === 0 && this.options[0].icon === 'heart') {
					uni.request({
						url: '/api/collect/addcollect',
						method: 'POST',
						data: {
							goodsId: this.id,
							openId: "oRrdQt3Tkr7_DfmIAlt9odmhH8kQ"
						},
						success: res => {
							console.log(res);
							if (res.data.data === "success") {
								this.options[0].icon = 'heart-filled'
								this.options[0].text = '取消收藏'
								uni.showToast({
									title: '收藏成功',
									icon: 'none'
								})
							}
						},
						fail: () => {},
						complete: () => {}
					});
					return
				}
				if (e.index === 0 && this.options[0].icon === 'heart-filled') {
					uni.request({
						url: `/api/goods/detailaction?id=${this.id}&openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
						method: 'GET',
						data: {},
						success: res => {
							console.log(res);
							this.options[0].icon = 'heart'
							this.options[0].text = '收藏'
							res.data.collected = false
							uni.showToast({
								title: '已取消收藏',
								icon: 'none'
							})
						},
						fail: () => {},
						complete: () => {}
					});
				}
			},
			// 点击商品导航栏右侧按钮
			buttonClick(e) {
				console.log(e)
				// 点击加入购物侧时
				if (e.index === 0) {
					this.show = true
				}
				if (e.index === 1) {
					this.shows = true
				}
			},
			// 步进器
			valChange(e) {
				// console.log('当前值为: ' + e.value)
				this.value = e.value
			},
			// 确定加入购物车
			add() {
				uni.request({
					// #ifdef  H5
					url: `/api/cart/addCart`,
					// #endif
					// #ifdef MP-WEIXIN || APP-PLUS
					url: `${this.$api}/cart/addCart`,
					// #endif
					method: 'POST',
					data: {
						goodsId: this.id,
						number: this.value,
						openId: "oRrdQt3Tkr7_DfmIAlt9odmhH8kQ"
					},
					success: res => {
						console.log(res);
						this.show = false
						this.options[1].info = this.value
						uni.showToast({
							title: '已添加到购物车',
							icon: 'none'
						})
					},
					fail: () => {},
					complete: () => {}
				});

			},
			//立即购买
			buy() {
				uni.request({
					// #ifdef H5
					url: `/api/order/submitAction`,
					// #endif
					// #ifdef MP-WEIXIN || APP-PLUS 
					url: `${this.$api}/order/submitAction`,
					// #endif
					method: 'POST',
					data: {
						allPrise: this.info.retail_price * this.value,
						goodsId: this.info.id,
						openId: "oRrdQt3Tkr7_DfmIAlt9odmhH8kQ",
					},
					success: res => {
						console.log(res);
						uni.navigateTo({
							url:`../buyGoods/buyGoods?flag=1`
						})
					},
					fail: () => {},
					complete: () => {}
				});
			},
			gotoDetail(item){
				// console.log(item);
				let id = item.id
				uni.navigateTo({
					url:`../goodsDetail/goodsDetail?id=${id}`
				})
			}
		},
		mounted() {

		},
		onLoad(options) {
			// console.log(options);
			this.id = options.id
			uni.showLoading({
				title: '加载中'
			})
			uni.request({
				// #ifdef H5
				url: `/api/goods/detailaction?id=${options.id}&openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
				// #endif
				// #ifdef MP-WEIXIN || APP-PLUS
				url: `${this.$api}/goods/detailaction?id=${options.id}&openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
				// #endif
				method: 'GET',
				data: {},
				success: res => {
					uni.hideLoading() //取消加载
					console.log(res);
					this.info = res.data.info //商品信息
					this.gallery = res.data.gallery //商品轮播图
					this.attribute = res.data.attribute //商品参数
					this.issue = res.data.issue //常见问题
					this.productList = res.data.productList //大家都在看
					if (res.data.collected === true) {
						this.options[0].icon = 'heart-filled'
						this.options[0].text = '取消收藏'
					}
				},
				fail: () => {},
				complete: () => {}
			});
		},
		filters: {

		},
		computed: {

		},
		watch: {

		},
		directives: {

		}
	}
</script>

<style scoped lang="scss">
	.yuan {
		width: 10rpx;
		height: 10rpx;
		margin-right: 10rpx;
	}

	.notes {
		background: rgb(238, 238, 238);
		font-size: 24rpx;
		height: 70rpx;
	}

	.info {
		height: 200rpx;
		font-size: 26rpx;
		border-bottom: 1rpx solid #c9c9c9;
	}

	.name {
		font-size: 34rpx;
		margin-top: 30rpx;
	}

	.price {
		color: rgb(162, 63, 63);
		margin-top: 50rpx;
	}

	.attributeTitle {
		font-size: 34rpx;
		margin-bottom: 40rpx;
	}

	.attributeAll {
		padding: 40rpx 25rpx;
	}

	.attributeOne {
		background: rgb(247, 247, 247);
		line-height: 70rpx;
		margin-bottom: 30rpx;
		padding: 0 20rpx;
	}

	.attributeName {
		width: 140rpx;
		font-size: 26rpx;
		margin-right: 40rpx;
		color: #999;
	}

	.desc {
		font-size: 0;
	}

	.dian {
		width: 30rpx;
		height: 30rpx;
	}

	.issueAll {
		padding: 40rpx 20rpx;
		font-size: 26rpx;
		color: #000;
	}

	.answer {
		color: #999;
		margin: 20rpx 20rpx;
	}

	.productList {
		width: 48%;
		border-top: 1rpx solid #c9c9c9;
		border-bottom: 1rpx solid #c9c9c9;
	}

	.list_pic_url {
		width: 100%;
		height: 250rpx;
	}

	.retail_price {
		color: rgb(162, 63, 63);
		margin: 20rpx 0;
	}

	.priceBottom {
		color: rgb(162, 63, 63);
		margin: 10rpx 0;
	}

	.productListAll {
		margin-top: 30rpx;
		padding: 0 20rpx;
		margin-bottom: 150rpx;
	}

	.border {
		border-right: 1rpx solid #c9c9c9;
	}

	.nav {
		position: fixed;
		bottom: 0;
		width: 100%;
	}

	.pic_url {
		width: 250rpx;
		height: 250rpx;
	}

	.num {
		padding: 0 60rpx;
		margin-bottom: 30rpx;
	}

	.numName {
		margin-right: 40rpx;
	}
</style>
