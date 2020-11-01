<template>
	<view class="All">
		<view class="box flex column alii-center jus-center" @click="gotoMore">
			<view class="first">
				新品首发
			</view>
			<view class="more">
				查看更多
			</view>
		</view>
		<view class="newGoodsAll">
			<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120">
				<view class="flex fwno">
					<view class="newOne" v-for="item in newGoods" :key="item.id" @click="gotoDetail(item)">
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
			newGoods: {
				type: Array
			}
		},
		data() {
			return {}
		},
		methods: {
			gotoMore() {
				uni.navigateTo({
					url: `../../pages/NewandHot/NewandHot?flag=0`
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
		background: rgb(241, 247, 252);
		color: rgb(155, 169, 188);
	}

	.more {
		background: rgb(216, 228, 240);
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
