<template>
	<view class="All">
		<view class="one flex alii-center" v-for="item in list" :key="item.id">
			<image :src="item.list_pic_url" mode="" class="pic"></image>
			<view class="">
				<view class="name flex juscb">
					<view class="">
						{{item.name}}
					</view>
					<view class="price">
						￥{{item.retail_price}}
					</view>
				</view>
				<view class="brief">
					{{item.goods_brief}}
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
				list: []
			}
		},
		methods: {

		},
		mounted() {

		},
		onLoad() {

		},
		onShow() {
			uni.request({
				// #ifdef H5
				url: `/api/collect/listAction?openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
				// #endif
				// #ifdef MP-WEIXIN || APP-PLUS 
				url: `${this.$api}/collect/listAction?openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
				// #endif
				method: 'GET',
				data: {},
				success: res => {
					console.log(res);
					this.list = res.data.collectGoodsList
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
	.pic {
		width: 200rpx;
		height: 200rpx;
	}
	.name{
		font-size: 30rpx;
		margin-bottom: 10rpx;
	}
	.price{
		color: rgb(180,40,45);
		margin-left: 20rpx;
	}
	.brief{
		color: #999;
	}
	.one{
		border-bottom: 1rpx solid #c9c9c9;
	}
	.All{
		padding: 0 20rpx;
	}
</style>
