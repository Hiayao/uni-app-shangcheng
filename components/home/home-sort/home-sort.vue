<template>
	<view class="All">
		<view class="newCategoryList" v-for="(item1,index1) in newCategoryList"  >
			<view class="fw6 tac">
				{{item1.name}}
			</view>
			<view class="list flex fwrap juscb">
				<view class="listOne tac" v-for="item in item1.goodsList" :key="item.id" @click="gotoDetail(item)">
					<image :src="item.list_pic_url" mode="" class="pic"></image>
					<view class="thidden-one">
						{{item.name}}
					</view>
					<view class="price">
						￥{{item.retail_price}}
					</view>
				</view>
				<view class="box flex alii-center jus-center" @click="gotoSort(item1,index1)">
					<view>
						{{item1.name}}
					</view>
					<view class="goAll borderr flex alii-center jus-center">
						<image src="../../../image/go.png" mode="" class="go"></image>
					</view>
				</view>
			</view>
			<view class="nobox">

			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "",
		components: {

		},
		props: {
			newCategoryList: {
				type: Array
			}
		},
		data() {
			return {}
		},
		methods: {
			gotoSort(item1,index1) {
				console.log(index1);
				let index = index1
				let id = item1.id
				uni.navigateTo({
					url: `../../pages/home/homeSort?id=${id}&index=${index}`
				})
			},
			gotoDetail(item){
				console.log(item);
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
		background: rgb(238, 238, 238);
		padding-top: 40rpx;
	}

	.list {
		padding: 0 20rpx;
	}

	.listOne {
		width: 48%;
		background: #fff;
		margin-top: 30rpx;
		height: 430rpx;
	}

	.pic {
		width: 100%;
		height: 340rpx;
	}

	.go {
		width: 40rpx;
		height: 40rpx;
	}

	.goAll {
		width: 60rpx;
		height: 60rpx;
		border: 1rpx solid #999;
		margin-left: 10rpx;
	}

	.box {
		width: 48%;
		background: #fff;
		height: 430rpx;
		margin-top: 30rpx;
	}

	.nobox {
		background: #fff;
		height: 40rpx;
		margin-top: 16rpx;
	}

	.price {
		color: rgb(162, 63, 63);
		margin-top: 10rpx;
		font-size: 26rpx;
	}
	.newCategoryList{
		margin-top: 20rpx;
	}
</style>
