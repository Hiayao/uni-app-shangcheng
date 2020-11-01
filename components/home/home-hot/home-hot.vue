<template>
	<view class="All">
		<view class="box flex column alii-center jus-center" @click="gotoMore">
			<view class="first">
				人气推荐·好物精选
			</view>
			<view class="more">
				查看更多
			</view>
		</view>
		<view class="newGoodsAll">
			<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120">
				<view class="flex fwno">
					<view class="newOne" v-for="item in hotGoods" :key="item.id" @click="gotoDetail(item)">
						<image :src="item.list_pic_url" mode="" class="pic"></image>
						<view class="name fw6 thidden-one">
							{{item.name}}
						</view>
						<view class="brief thidden-one">
							{{item.goods_brief}}
						</view>
						<view class="price">
							￥{{item.retail_price}}
						</view>
					</view>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "",
		components: {

		},
		props: {
			hotGoods: {
				type: Array
			}
		},
		data() {
			return {}
		},
		methods: {
			gotoMore() {
				uni.navigateTo({
					url: `../../pages/NewandHot/NewandHot?flag=1`
				})
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
		onLoad() {

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
	.All {
		margin-top: 40rpx;
		width: 100%;
	}

	.box {
		width: 100%;
		height: 260rpx;
		background: rgb(252, 244, 221);
		color: rgb(193, 170, 122);
	}

	.more {
		background: rgb(243, 233, 203);
		padding: 10rpx 30rpx;
		width: 200rpx;
		text-align: center;
	}

	.first {
		font-size: 34rpx;
		margin-bottom: 20rpx;
	}

	.pic {
		width: 300rpx;
		height: 300rpx;
	}

	.name {
		font-size: 30rpx;
	}

	.brief {
		color: #999;
		margin: 10rpx 0;
	}

	.price {
		color: rgb(162, 63, 63);
	}

	.newOne {
		margin-right: 40rpx;
	}

	.newGoodsAll {
		padding: 20rpx;
	}
</style>
