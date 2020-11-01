<template>
	<view class="All">
		<view class="tac fw6">
			专题精选
		</view>
		<view class="topicList">
			<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120">
				<view class="flex fwno">
					<view class="topicListOne" v-for="item in topicList" :key="item.id">
						<image :src="item.item_pic_url" mode="" class="pic"></image>
						<view class="flex">
							<view class="">
								{{item.title}}
							</view>
							<view class="price">
								￥{{item.price_info}}起
							</view>
						</view>
						<view class="subtitle thidden-one">
							{{item.subtitle}}
						</view>
					</view>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "",
		components: {

		},
		props: {
			topicList: {
				type: Array
			}
		},
		data() {
			return {}
		},
		methods: {

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
	.All {
		margin-top: 40rpx;
		padding: 20rpx;
	}

	.tac {
		margin-bottom: 20rpx;
	}

	.pic {
		height: 320rpx;
		width: 600rpx;
		margin-bottom: 10rpx;
	}

	.topicList {
		color: #000;
	}
	.topicListOne{
		margin-right: 20rpx;
	}
	.price{
		margin-left: 20rpx;
		color: rgb(162,63,63);
	}
	.subtitle{
		color: #999;
		font-size: 26rpx;
		margin-top: 20rpx;
	}
</style>
