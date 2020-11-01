<template>
	<view>
		<view class="">
			<u-swipe-action :show="item.show" :index="index" v-for="(item, index) in list" :key="item.id" @click="click" @open="open"
			 :options="options">
				<view class="listOne flex column jus-center" @click="sureRess(item)">
					<view class="flex alii-center">
						<view class="">
							{{item.name}}
						</view>
						<view class="mobile">
							{{item.mobile}}
						</view>
						<view class="moren" v-if="item.is_default ===1">
							默认
						</view>
					</view>
					<!-- 此层wrap在此为必写的，否则可能会出现标题定位错误 -->
					<view class="title-wrap">
						{{item.address}}{{item.address_detail}}
					</view>
				</view>


			</u-swipe-action>
		</view>

		<view class="btn">
			<u-button type="primary" @click='gotoAdd'>添加地址</u-button>
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
				disabled: false,
				btnWidth: 180,
				show: false,
				flag:'', //从支付页传来的参数
				options: [{
						text: '修改',
						style: {
							backgroundColor: '#007aff'
						}
					},
					{
						text: '删除',
						style: {
							backgroundColor: '#dd524d'
						}
					}
				]
			}
		},
		methods: {
			// 去添加地址
			gotoAdd() {
				uni.navigateTo({
					url: '../address/address'
				})
			},
			// 获取地址列表
			getList() {
				uni.request({
					// #ifdef H5
					url: `/api/address/getListAction?openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
					// #endif
					// #ifdef MP-WEIXIN || APP-PLUS 
					url: `${this.$api}/address/getListAction?openId=oRrdQt3Tkr7_DfmIAlt9odmhH8kQ`,
					// #endif
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
			// 选择修改或删除
			click(index, index1) {
				if (index1 == 1) {
					this.list.splice(index, 1);
					this.$u.toast(`删除了第${index}个cell`);
				}if (index1 == 0) {
					let lists = JSON.stringify(this.list[index])
					console.log(lists);
					uni.navigateTo({
						url:`../address/address?list=${lists}`
					})
				} 
			},
			// 如果打开一个的时候，不需要关闭其他，则无需实现本方法
			open(index) {
				// 先将正在被操作的swipeAction标记为打开状态，否则由于props的特性限制，
				// 原本为'false'，再次设置为'false'会无效
				this.list[index].show = true;
				this.list.map((val, idx) => {
					if (index != idx) this.list[idx].show = false;
				})
			},
			sureRess(item){
				console.log(item);
				if(this.flag === '1'){
				uni.request({
					// #ifdef H5
					url: `/api/address/saveAction`,
					// #endif
					// #ifdef MP-WEIXIN || APP-PLUS 
					url: `${this.$api}/address/saveAction`,
					// #endif
					method: 'POST',
					data: {
						address: item.address,
						addressId: item.id,
						checked: true,
						detailadress: item.address_detail,
						openId: "oRrdQt3Tkr7_DfmIAlt9odmhH8kQ",
						telNumber: item.mobile,
						userName: item.name
					},
					success: res => {
						console.log(res);
						uni.navigateBack({
							delta:1
						})
					},
					fail: () => {},
					complete: () => {}
				});
				}
			},
		},
		mounted() {

		},
		onLoad(options) {
		this.flag = options.flag
		},
		onShow() {
			this.getList()
			this.show = false
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
	.btn {
		position: fixed;
		bottom: 0;
		width: 80%;
		margin-left: 10%;
	}

	.item {
		padding: 20rpx;
	}

	.title-wrap{
		margin-top: 20rpx;
	}

	.title {
		text-align: left;
		font-size: 28rpx;
		color: $u-content-color;
		margin-top: 20rpx;
	}

	.listOne {
		height: 140rpx;
		padding: 0 30rpx;
	}
	.moren{
		padding: 5rpx 20rpx;
		background: red;
		color: #fff;
	}
	.mobile{
		margin: 0 20rpx;
	}
</style>
