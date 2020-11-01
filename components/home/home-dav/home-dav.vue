<template>
	<view>
		<view class="flex jusca mt20">
			<view class="flex column alii-center" v-for="item in dav" :key="item.id" @click="gotoDetail(item)">
				<image :src="item.icon_url" mode="" class="img"></image>
				<view class="">
					{{item.name}}
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
		props: {
			dav: {
				type: Array
			}
		},
		data() {
			return {

			}
		},
		methods: {
			gotoDetail(item) {
				uni.request({
					url: `/api/category/currentaction?id=${item.id}`,
					method: 'GET',
					data: {},
					success: res => {
						console.log(res);
						let id = res.data.data.currentOne.subList[0].id
						let index = 0
						uni.navigateTo({
							url: `../../pages/home/homeSort?id=${id}&index=${index}`
						})
					},
					fail: () => {},
					complete: () => {}
				});
				
			},
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
	.img {
		width: 70rpx;
		height: 70rpx;
		margin-bottom: 10rpx;
	}
</style>
