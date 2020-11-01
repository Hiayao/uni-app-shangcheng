<template>
	<view>
		<image src="../../image/caitiao.jpg" mode="" class="caitiao"></image>
		<view class="addressBox">
			<view class="tac noRess" v-if="!address.id" @click="gotoAddress">
				去添加地址
			</view>
			<view class="RessAll flex alii-center juscb" v-if="address.id">
				<view class="Ress flex column jus-center ">
					<view class="">
						{{address.name}} {{address.mobile}}
					</view>
					<view class="">
						{{address.address}}{{address.address_detail}}
					</view>
				</view>
				<image src="../../image/go.png" mode="" class="go" @click="gotoAddress"></image>
			</view>
		</view>

		<view class="info">
			<view class="infoOne flex juscb alii-center">
				<view class="">
					商品合计
				</view>
				<view class="">
					￥{{this.total}}
				</view>
			</view>
			<view class="infoOnes flex juscb alii-center">
				<view class="">
					运费
				</view>
				<view class="">
					免运费
				</view>
			</view>
			<view class="infoOne flex juscb alii-center">
				<view class="">
					优惠券
				</view>
				<view class="">
					暂无
				</view>
			</view>
		</view>

		<view class="listAll">
			<view class="flex alii-center listOne juscb" v-for="item in list" :key="item.id">
				<view class="flex alii-center ">
					<image :src="item.list_pic_url" class="image" />
					<view class="">
						<view class="title">{{ item.goods_name }}</view>
						<view class="">
							￥{{item.retail_price}}
						</view>
					</view>
				</view>
				<view class="num" v-if="flag ===''">
					X {{item.number}}
				</view>
				<view class="num" v-if="flag ==='1'">
					X {{total/item.retail_price}}
				</view>
			</view>
		</view>

		<view class="sureAll flex alii-center juscb">
			<view class="">
				实付：￥{{this.total}}
			</view>
			<view class="sure flex alii-center jus-center fw6" @click="sureBuy">
				支付
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
				list: [], //准备下单的商品数组
				total: 0, //准备下单的商品总价
				id: [], //购买的商品id集合
				flag: '', //从立即购买传来的参数
				address: {}, //收货地址
			}
		},
		methods: {
			sureBuy() {
				uni.showToast({
					title: '成功支付' + this.total + '元',
					icon: 'success'
				})
				setTimeout(() => {
					uni.switchTab({
						url: '../home/home'
					})
				}, 2000)
				this.list.map(item => {
					let id = item.id
					uni.request({
						// #ifdef H5
						url: `/api/cart/deleteAction?id=${id}`,
						// #endif
						// #ifdef MP-WEIXIN || APP-PLUS
						url: `${this.$api}/cart/deleteAction?id=${id}`,
						// #endif
						method: 'GET',
						data: {},
						success: res => {
							console.log(res);
						},
						fail: () => {},
						complete: () => {}
					});
				})
				
			},
			// 获取默认地址
			getRess() {
				uni.request({
					// #ifdef H5
					url: `/api/address/getListAction?openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
					// #endif
					// #ifdef MP-WEIXIN || APP-PLUS 
					url: `${this.$api}/address/getListAction?openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
					// #endif
					method: 'GET',
					data: {},
					success: res => {
						console.log(res);
						let ress = res.data.data
						console.log(ress);
						ress.map(item => {
							if (item.is_default === 1) {
								this.address = item
							}
						})
						console.log(this.address);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 选择地址
			gotoAddress(){
				uni.navigateTo({
					url:`../addressList/addressList?flag=1`
				})
			},
		},
		mounted() {

		},
		onLoad(options) {
			console.log(options);
			if (options.total) {
				this.list = JSON.parse(options.arr)
				this.total = options.total
				console.log(this.list);
				this.list.map(item => {
					this.id.push(item.goods_id)
				})
				console.log(this.id);
			}
			if (options.flag) {
				this.flag = options.flag
				uni.request({
					// #ifdef H5
					url: `/api/order/detailAction?openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ&addressId=70`,
					// #endif
					// #ifdef MP-WEIXIN || APP-PLUS 
					url: `${this.$api}/order/detailAction?openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ&addressId=70`,
					// #endif
					method: 'GET',
					data: {},
					success: res => {
						console.log(res);
						this.list = res.data.goodsList
						this.total = res.data.allPrise
					},
					fail: () => {},
					complete: () => {}
				});
			}

		},
		onShow() {
			this.getRess()
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
	page {
		background: rgb(244, 244, 244);
	}

	.caitiao {
		width: 100%;
		height: 36rpx;
	}

	.addressBox {
		height: 200rpx;
		background: #fff;
	}

	.noRess {
		line-height: 200rpx;
		text-decoration: underline
	}

	.Ress {
		height: 100%;
	}

	.RessAll {
		height: 100%;
		padding: 0 30rpx;
	}

	.info {
		background: #fff;
		padding: 0 30rpx;
		margin-top: 20rpx;
	}

	.infoOne {
		height: 100rpx;
	}

	.infoOnes {
		height: 100rpx;
		border-bottom: 1rpx solid #c9c9c9;
		border-top: 1rpx solid #c9c9c9;
	}

	.listAll {
		background: #fff;
		padding: 0 10rpx;
		margin-top: 20rpx;
	}

	.image {
		width: 140rpx;
		// flex: 0 0 120rpx;
		height: 140rpx;
		margin-right: 20rpx;
		border-radius: 12rpx;
	}

	.listOne {
		border-bottom: 1rpx solid #c9c9c9;
	}

	.title {
		margin-bottom: 10rpx;
	}

	.num {
		margin-right: 20rpx;
	}

	.sureAll {
		height: 100rpx;
		background: #fff;
		position: fixed;
		bottom: 0;
		width: 100%;
		padding-left: 20rpx;
	}

	.sure {
		height: 100%;
		background: rgb(180, 40, 45);
		width: 160rpx;
		color: #fff;
		margin-left: 20rpx;
	}
	.go{
		width: 40rpx;
		height: 40rpx;
	}
</style>
