<template>
	<view class="All" v-if="dav.length > 0">
		<u-tabs ref="tabs" :list="dav" active-color="#A23F3F" inactive-color="#606266" @change="change" font-size="30"
		 :current="current"></u-tabs>
		<view class="dav tac flex column jus-center">
			<view class="name">
				{{dav[fleg].name}}
			</view>
			<view class="front">
				{{dav[fleg].front_desc}}
			</view>
		</view>
		<view class="ListAll flex fwrap juscb">
			<view class="sortList" v-for="(item1,index1) in sortList" :key="item1.id" @click="gotoDetail(item1)">
				<image :src="item1.list_pic_url" mode="" class="pic"></image>
				<view class="tac">
					{{item1.name}}
				</view>
				<view class="tac price">
					￥{{item1.retail_price}}
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
				dav: [], //分类导航
				current:0,
				fleg: 0,
				sortList: [], //分类的商品
				listTitle: {}, //分类的title
				id: '',
				
			}
		},
		methods: {
			// 获取首页分类导航
			getDav() {
				uni.request({
					url: `/api/category/categoryNav?id=${this.id}`,
					method: 'GET',
					data: {},
					success: res => {
						console.log(res);
						this.dav = res.data.navData
						this.dev = res.data.navData[this.fleg]
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 分类导航商品列表
			getSort() {
				uni.showLoading({
					title:'加载中'
				})
				uni.request({
					url: `/api/goods/goodsList?categoryId=${this.id}`,
					method: 'GET',
					data: {},
					success: res => {
						uni.hideLoading()//取消加载
						console.log(res);
						this.sortList = res.data.data
						this.listTitle = res.data.currentNav
					},
					fail: () => {},
					complete: () => {}
				});
			},
			change(index) {
				this.current = index;
				this.fleg = index
				this.dav.map((item, index) => {
					if (index === this.current) {
						this.id = item.id
					}
				})
				this.getSort()
			},
			gotoDetail(item1){
				let id = item1.id
				uni.navigateTo({
					url:`../goodsDetail/goodsDetail?id=${id}`
				})
			}
		},
		mounted() {
			// this.getDav()
			this.getSort()
		},
		onLoad(options) {
			console.log(options);
			this.id = options.id
			this.current = options.index
			console.log(this.current);
			this.fleg = options.index
			this.getDav()
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
		background: rgb(244, 244, 244);
	}

	.dav {
		background: #fff;
		margin-top: 20rpx;
		height: 160rpx;
	}

	.name {
		margin-bottom: 20rpx;
		font-size: 36rpx;
	}

	.front {
		color: #999;
	}

	.ListAll {
		margin-top: 10rpx;
	}

	.sortList {
		background: #fff;
		width: 49%;
		margin-bottom: 10rpx;
	}

	.pic {
		width: 100%;
		height: 340rpx;
	}
	.price{
		color: rgb(180,40,45);
		font-size: 30rpx;
		margin-top: 10rpx;
	}
</style>
