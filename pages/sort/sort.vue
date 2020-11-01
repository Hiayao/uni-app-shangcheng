<template>
	<view>
		<!-- <category-list :height="400" :defaultActive="2" :offsetTop="200" :categoryList="categoryList" @categoryMainClick="categoryMainClick"
               @categorySubClick="categorySubClick"></category-list> -->
		<view class="ipt">
			<u-search placeholder="日照香炉生紫烟" @focus="focus" :show-action="false" v-model="keyword"></u-search>
		</view>

		<view class="listAll flex">
			<view class="f1">
				<view class="dav tac" :class="flag === index?'red':''" v-for="(item,index) in categoryList" :key="item.id" @click="inOne(item,index)">
					{{item.name}}
				</view>
			</view>

			<view class="f3">
				<view class="imgAll">
					<image :src="nowList.banner_url" mode="" class="img"></image>
					<view class="box flex alii-center jus-center">
						{{nowList.front_desc}}
					</view>
				</view>
				<view class="tac name">
					—— {{nowList.name}}分类 ——
				</view>
				<view class="flex fwrap subListAll juscb">
					<view class="subList" v-for="(item,index) in nowList.subList" @click="gotoDetail(item,index)">
						<image :src="item.wap_banner_url" mode="" class="wap_banner_url"></image>
						<view class="tac">
							{{item.name}}
						</view>
					</view>
				</view>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		name: "",
		components: {},
		props: {},
		data() {
			return {
				categoryList: [], //分类列表
				nowList: [], //当前分类
				// currentOne:{},
				keyword: '',
				id: '',
				flag: 0
			}
		},
		methods: {
			getData() {
				// 分类列表
				uni.request({
					// #ifdef H5
					url: '/api/category/indexaction',
					// #endif
					// #ifdef MP-WEIXIN || APP-PLUS
					url: '${this.$api}/category/indexaction',
					// #endif
					method: 'GET',
					data: {},
					success: res => {
						console.log(res);
						this.categoryList = res.data.categoryList
						this.id = res.data.categoryList[0].id
						this.getList()
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getList() {
				// 当前分类
				uni.showLoading({
					title:'加载中'
				})
				uni.request({
					// #ifdef H5
					url: `/api/category/currentaction?id=${this.id}`,
					// #endif
					// #ifdef MP-WEIXIN || APP-PLUS
					url: `${this.$api}/category/currentaction?id=${this.id}`,
					// #endif
					method: 'GET',
					data: {},
					success: res => {
						uni.hideLoading()//取消加载
						console.log(res); 
						this.nowList = res.data.data.currentOne
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 点击侧边栏的每一项
			inOne(item, index) {
				this.flag = index
				this.id = item.id
				console.log(this.id);
				this.getList()
			},
			// 点击跳转小分类详情
			gotoDetail(item, index) {
				uni.navigateTo({
					url: `../home/homeSort?id=${item.id}&index=${index}`
				})
			},
			// 搜索框获取焦点
			focus() {
				uni.navigateTo({
					url: '../../pages/search/search'
				})
			},
		},
		mounted() {
			this.getData()

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
	.ipt {
		line-height: 100rpx;
		width: 90%;
		margin-left: 5%;
	}

	.listAll {
		margin-top: 20rpx;
	}

	.dav {
		width: 130rpx;
		line-height: 100rpx;
	}

	.img {
		width: 90%;
		height: 200rpx;
	}

	.imgAll {
		width: 100%;
		position: relative;
	}

	.box {
		width: 90%;
		height: 100%;
		position: absolute;
		top: 0;
		background: rgba(0, 0, 0, .2);
		color: #fff;
		font-size: 28rpx;
	}

	.name {
		width: 90%;
		margin-top: 20rpx;
	}

	.wap_banner_url {
		width: 100%;
		height: 150rpx;
	}

	.subListAll {
		width: 90%;
	}

	.subList {
		width: 30%;
		margin-bottom: 30rpx;
	}

	.red {
		border-left: 4rpx solid #FF0000;
		font-size: 32rpx;
	}
</style>
