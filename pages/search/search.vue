<template>
	<view class="All">
		<view class="search">
			<u-search placeholder="日照香炉生紫烟" v-model="keyword" :clearabled="true" :show-action="true" action-text="取消" @search="search"
			 @change="change" @custom="cancel"></u-search>
		</view>
		<view class="" v-if="fleg === false">
			<view class="">
				<view class="flex juscb">
					<view class="title">
						搜索历史
					</view>
					<image src="../../image/lese.png" mode="" class="lese" @click="del"></image>
				</view>
				<view class="history flex fwrap">
					<view class="historyOne" v-for="item in historyData" :key="item.id" v-if="historyData.length > 0" @click="again(item)">
						{{item.keyword}}
					</view>
					<view class="tac" v-if="historyData.length === 0">
						暂无搜索历史
					</view>
				</view>
			</view>

			<view class="hotAll">
				<view class="title">
					热门搜索
				</view>
				<view class="history flex fwrap">
					<view class="redHot">
						{{redHot.keyword}}
					</view>
					<view class="historyOne" v-for="item in hotList" :key="item.id" @click="again(item)">
						{{item.keyword}}
					</view>
				</view>
			</view>
		</view>

		<view class="" v-if="fleg === true">
			<view class="searchList" v-for="item in searchList" :key="item.id" v-if="searchList.length > 0" @click="gotoDetail(item)">
				{{item.name}}
			</view>
			<view class="" v-if="searchList.length === 0">
				暂时没有您搜索的产品哦
			</view>
		</view>
		<uni-popup ref="popup" type="dialog">
			<uni-popup-dialog type="input" message="成功消息" :duration="2000" :before-close="true" @close="close" @confirm="confirm" content="确认删除全部历史记录吗？"></uni-popup-dialog>
		</uni-popup>
	</view>
</template>

<script>
	import uniPopup from '@/components/uni-popup/uni-popup.vue'
	import uniPopupMessage from '@/components/uni-popup/uni-popup-message.vue'
	import uniPopupDialog from '@/components/uni-popup/uni-popup-dialog.vue'
	export default {
		name: "",
		components: {
			uniPopup,
			uniPopupMessage,
			uniPopupDialog
		},
		props: {},
		data() {
			return {
				keyword: '',
				hotList: [], //热门搜索词组
				redHot: '', //高亮的热门搜索词
				openId: "oRrdQt3Tkr7_DfmIAlt9odmhH8kQ",
				historyData: [], //搜索历史
				searchList: [], //搜索数据
				fleg: false, //是否确认搜索触发的中间变量
			}
		},
		methods: {
			// 获取搜索的各种数据
			getData() {
				uni.request({
					url: `/api/search/indexaction?openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
					method: 'GET',
					data: {},
					success: res => {
						console.log(res);
						this.redHot = res.data.defaultKeyword
						this.hotList = res.data.hotKeywordList.splice(1)
						this.historyData = res.data.historyData.reverse()
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 点击取消回到首页
			cancel() {
				if (this.keyword === '') {
					// uni.switchTab({
					// 	url: '../home/home'
					// })
					uni.navigateBack({
						delta:1
					})
				}
				if (this.keyword !== '') {
					this.keyword = ''
				}
			},

			// 获取搜索数据
			getList() {
				uni.request({
					url: `/api/search/helperaction?keyword=${this.keyword}`,
					method: 'GET',
					data: {},
					success: res => {
						console.log(res);
						this.searchList = res.data.keywords
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 确定搜索事件
			search() {
				if (this.keyword !== '') {
					//添加历史记录
					uni.request({
						url: `/api/search/addhistoryaction`,
						method: 'POST',
						data: {
							keyword: this.keyword,
							openId: this.openId
						},
						success: res => {
							console.log(res);
							this.getData()
						},
						fail: () => {},
						complete: () => {}
					});
					this.getList()
					this.fleg = true
				}
			},
			// 搜索框输入事件
			change(e) {
				// console.log(e);
				if (e !== '') {
					this.keyword = e
				}
				if (e === '') {
					this.fleg = false
				}
			},
			// 清空搜索历史词
			del() {
				this.$refs.popup.open()
				
			},
			//点击搜索词再次搜索
			again(item) {
				console.log(item);
				this.keyword = item.keyword
				this.search()
			},
			close(done) {
				done()
			},
			confirm(done, value) {
				console.log(value)
				done()
				uni.request({
					url: `/api/search/clearhistoryAction`,
					method: 'POST',
					data: {
						openId: this.openId
					},
					success: res => {
						console.log(res)
						this.getData()
					},
					fail: () => {},
					complete: () => {}
				});
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
			this.getData()
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
	.search {
		line-height: 120rpx;
	}

	.All {
		padding: 0 20rpx;
	}

	.lese {
		width: 30rpx;
		height: 30rpx;
		margin-right: 20rpx;
	}

	.historyOne {
		border: 1rpx solid #999;
		padding: 10rpx;
		margin-right: 30rpx;
		margin-bottom: 30rpx;
	}

	.redHot {
		border: 1rpx solid red;
		padding: 10rpx;
		margin-right: 30rpx;
		color: red;
		margin-bottom: 30rpx;
	}

	.history {
		margin-top: 30rpx;
	}

	.hotAll {
		margin-top: 10rpx;
	}

	.title {
		font-size: 32rpx;
	}

	.tac {
		width: 100%;
	}

	.searchList {
		margin-bottom: 40rpx;
	}
</style>
