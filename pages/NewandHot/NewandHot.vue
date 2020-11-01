<template>
	<view class="All">
		<view class="imgAll pore">
			<image src="../../image/backg.png" mode="" class="img"></image>
			<view class="poab">
			</view>
		</view>
		<view class="dav flex alii-center jusca">
			<view :class="fleg === 0 ?'red':''" @click="zonghe">
				综合
			</view>
			<view class="flex alii-center">
				<view :class="fleg === 1 ?'red':''" @click="jiage">
					价格
				</view>
				<view class="icoAll">
					<view class="ico">
						<image src="../../image/up.png" mode="" class="ico" @click="up"></image>
					</view>
					<view class="ico">
						<image src="../../image/down.png" mode="" class="ico" @click="down"></image>
					</view>
				</view>
			</view>
			<view :class="fleg === 2 ?'red':''" @click="fenlei">
				分类
			</view>
		</view>

		<view class="listAll flex fwrap">
			<view class="list" v-for="item in list" :key="item.id">
				<image :src="item.list_pic_url" mode="" class="pic"></image>
				<view class="tac">
					{{item.name}}
				</view>
				<view class="tac price">
					￥{{item.retail_price}}
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
				list: [],
				fleg:0,  //点击dav的样式判断
				flag:0,  //跳转到当前页面传来的判断新品和人气的值
			}
		},
		methods: {
			// 新品综合排序
			getNew() {
				uni.request({
					url: '/api/goods/goodsList?isNew=1',
					method: 'GET',
					data: {},
					success: res => {
						// console.log(res);
						this.list = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 新品分类排列
			fenNew(){
				uni.request({
					url: '/api/goods/goodsList?isNew=1',
					method: 'GET',
					data: {},
					success: res => {
						// console.log(res);
						this.list = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 新品价格升序
			upNew(){
				uni.request({
					url: '/api/goods/goodsList?isNew=1&order=asc',
					method: 'GET',
					data: {},
					success: res => {
						// console.log(res);
						this.list = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 新品价格降序
			downNew(){
				uni.request({
					url: '/api/goods/goodsList?isNew=1&order=desc',
					method: 'GET',
					data: {},
					success: res => {
						this.list = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 人气综合排序
			getHot() {
				uni.request({
					url: '/api/goods/goodsList?isHot=1',
					method: 'GET',
					data: {},
					success: res => {
						// console.log(res);
						this.list = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 人气分类排列
			fenHot(){
				uni.request({
					url: '/api/goods/goodsList?isHot=1',
					method: 'GET',
					data: {},
					success: res => {
						// console.log(res);
						this.list = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 人气价格升序
			upHot(){
				uni.request({
					url: '/api/goods/goodsList?isHot=1&order=asc',
					method: 'GET',
					data: {},
					success: res => {
						// console.log(res);
						this.list = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 人气价格降序
			downHot(){
				uni.request({
					url: '/api/goods/goodsList?isHot=1&order=desc',
					method: 'GET',
					data: {},
					success: res => {
						this.list = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 点击综合
			zonghe(){
				this.fleg = 0
				if(this.flag === '0'){
					this.getNew()
				}
				if(this.flag === '1'){
					this.getHot()
				}
			},
			// 点击价格
			jiage(){
				this.fleg = 1
			},
			// 点击分类
			fenlei(){
				this.fleg = 2
				if(this.flag === '0'){
					this.fenNew()
				}
				if(this.flag === '1'){
					this.fenHot()
				}
			},
			// 价格升序
			up(){
				this.fleg = 1
				if(this.flag === '0'){
					this.upNew()
				}
				if(this.flag === '1'){
					this.upHot()
				}
			},
			// 价格降序
			down(){
				this.fleg = 1
				if(this.flag === '0'){
					this.downNew()
				}
				if(this.flag === '1'){
					this.downHot()
				}
			},
		},
		mounted() {

		},
		onLoad(options) {
			// console.log(options.flag);
			this.flag = options.flag
			if (options.flag === '0') {
				this.getNew()
			}
			if (options.flag === '1') {
				this.getHot()
			}
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
	.imgAll {
		width: 100%;
		height: 350rpx;
	}

	.img {
		width: 100%;
		height: 100%;
	}

	.poab {
		width: 100%;
		height: 100%;
		top: 0;
		background: rgba(0, 0, 0, .2);
	}

	.ico {
		width: 30rpx;
		height: 30rpx;
	}

	.dav {
		height: 80rpx;
		background: #fff;
	}

	.icoAll {
		margin-left: 15rpx;
	}

	.All {
		background: rgb(238, 238, 238);
	}

	.listAll {
		margin-top: 20rpx;
		padding: 0 0 0 2%;
	}

	.list {
		width: 48%;
		background: #fff;
		margin-bottom: 20rpx;
		margin-right: 2%;
	}

	.pic {
		width: 100%;
		height: 300rpx;
	}

	.price {
		color: rgb(162, 63, 63);
		margin-top: 10rpx;
	}
	.red{
		color: rgb(162, 63, 63);
	}
</style>
