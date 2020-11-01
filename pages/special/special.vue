<template>
	<view>
		<view class="list" v-for="item in list" :key="item.id" @click="gotoDetail(item)">
			<image :src="item.scene_pic_url" mode="" class="pic"></image>
			<view class="tac">
				{{item.title}}
			</view>
			<view class="tac subtitle">
				{{item.subtitle}}
			</view>
			<view class="tac price">
				{{item.price_info}}元起
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
				list: [],
				page:1
			}
		},
		methods: {
			getData() {
				uni.request({
					url: `/api/topic/listaction?page=${this.page}`,
					method: 'GET',
					data: {},
					success: res => {
						console.log(res);
						this.list = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			gotoDetail(item){
				console.log(item);
				let id = item.id
				uni.navigateTo({
					url:`../specialDetail/specialDetail?id=${id}`
				})
			},
		},
		mounted() {
			this.getData()
		},
		onReachBottom(e) {
			// console.log('触底加载更多');
			this.page = this.page + 1
			uni.request({
				url: `/api/topic/listaction?page=${this.page}`,
				method: 'GET',
				data: {},
				success: res => {
					console.log(res);
					this.list = this.list.concat(res.data.data)
					console.log(this.list);
				},
				fail: () => {},
				complete: () => {}
			});
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
	.list {
		width: 100%;
		// height: 400rpx;
		margin-bottom: 30rpx;
	}

	.pic {
		width: 100%;
		height: 400rpx;
	}
	.subtitle{
		font-size: 26rpx;
		color: #999;
		margin: 10rpx 0;
	}
	.price{
		font-size: 26rpx;
		color: rgb(162, 63, 63);
	}
</style>
