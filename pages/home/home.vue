<template>
	<view>
		<!-- 搜索和定位 -->
		<homeLocation></homeLocation>
		<!-- 轮播图 -->
		<homeSwiper :banner="banner"></homeSwiper>
		<!-- 分类导航 -->
		<homeDav :dav="dav"></homeDav>
		<!-- 品牌直供 -->
		<homeBrand :brandList="brandList"></homeBrand>
		<!-- 新品 -->
		<homeNew :newGoods="newGoods"></homeNew>
		<!-- 人气推荐 -->
		<homeHot :hotGoods="hotGoods"></homeHot>
		<!-- 专题精选 -->
		<homeTopic :topicList="topicList"></homeTopic>

		<!-- 首页分类 -->
		<homeSort :newCategoryList="homeSort"></homeSort>
	</view>
</template>

<script>
	import homeLocation from '../../components/home/home-location/home-location.vue'
	import homeSwiper from '../../components/home/home-swiper/home-swiper.vue'
	import homeDav from '../../components/home/home-dav/home-dav.vue'
	import homeBrand from '../../components/home/home-brand/home-brand.vue'
	import homeNew from '../../components/home/home-new/home-new.vue'
	import homeHot from '../../components/home/home-hot/home-hot.vue'
	import homeTopic from '../../components/home/home-topic/home-topic.vue'
	import homeSort from '../../components/home/home-sort/home-sort.vue'
	export default {
		name: "",
		components: {
			homeLocation,
			homeSwiper,
			homeDav,
			homeBrand,
			homeNew,
			homeHot,
			homeTopic,
			homeSort
		},
		props: {},
		data() {
			return {
				banner: [], //首页轮播图
				dav: [], //首页分类导航
				brandList: [], //首页品牌直供
				newGoods: [], //首页新品
				hotGoods: [], //首页人气推荐
				topicList: [], //专题精选
				homeSort: [], //首页分类展示
				// address:'',
			}
		},
		methods: {

		},
		mounted() {
			uni.showLoading({
				title:'加载中'
			})
			uni.request({
				// #ifdef H5
				url: '/api/index/index',
				// #endif
				// #ifdef MP-WEIXIN || APP-PLUS
				url: `${this.$api}/index/index`,
				// #endif
				method: 'GET',
				data: {},
				success: res => {
					console.log(res);
					uni.hideLoading()//取消加载
					this.banner = res.data.banner //首页轮播图
					this.dav = res.data.channel //首页分类导航
					this.brandList = res.data.brandList //首页品牌直供
					this.newGoods = res.data.newGoods //首页新品
					this.hotGoods = res.data.hotGoods //首页人气推荐
					this.topicList = res.data.topicList //首页专题精选
					this.homeSort = res.data.newCategoryList //首页分类展示
				},
				fail: () => {},
				complete: () => {}
			});
			
			uni.getLocation({
			    type: 'gcj02', //返回可以用于uni.openLocation的经纬度
			    success: function (res) {
					console.log(res);
			        const latitude = res.latitude;
			        const longitude = res.longitude;
			        uni.openLocation({
			            latitude: latitude,
			            longitude: longitude,
			            success: function () {
			                console.log('success');
			            }
			        });
			    }
			});
			
		},
		onLoad(options) {
			// if(options.address){
			// 	this.address = options.address
			// }
		},
		filters: {

		},
		onShow() {
			
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

</style>
