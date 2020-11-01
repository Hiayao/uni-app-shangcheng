<template>
	<view>
		<view class="flex top alii-center">
			
			<!-- #ifdef H5 -->
			<view class="f1 flex jus-center" @click="gotoMap" v-if="address ===''">
				手动选择定位
			</view>
			<view class="f1 flex jus-center" @click="gotoMap" v-if="address !==''">
				{{address}}
			</view>
			<!-- #endif -->
			<view class="f2">
				<u-search placeholder="日照香炉生紫烟" v-model="keyword" @focus="focus"></u-search>
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

		},
		data() {
			return {
				keyword: '',
				address: '',
			}
		},
		methods: {
			focus() {
				uni.navigateTo({
					url: '../../pages/search/search'
				})
			},
			gotoMap() {
				uni.chooseLocation({
					success: (res) => {
						console.log('位置名称：' + res.name);
						console.log('详细地址：' + res.address);
						console.log('纬度：' + res.latitude);
						console.log('经度：' + res.longitude);
						this.address = res.address
						console.log(this.address);
					}
				});
			},
		},
		mounted() {
		uni.getLocation({
			type: 'gcj02 ',
			success: function(res) {
				console.log('当前位置的经度：' + res.longitude);
				console.log('当前位置的纬度：' + res.latitude);
			}
		});
		},
		onLoad() {
			
		},
		onShow() {

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
	.top {
		height: 100rpx;
	}
</style>
