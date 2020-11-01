<template>
	<view class="All">
		<view class="fw7 tac title ">
			品牌制造商直供
		</view>
		<view class="flex fwrap juscb">
			<view class="brandList" v-for="item in brandList" :key="item.id">
				<image :src="item.new_pic_url" mode="" class="pic"></image>
				<view class="nameAll">
					<view class="">
						{{item.name}}
					</view>
					<view class="price">
						{{item.floor_price}}元起
					</view>
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
			brandList: {
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
	.title {
		font-size: 34rpx;
		margin-bottom: 20rpx;
	}

	.All {
		margin-top: 50rpx;
		padding: 0 20rpx
	}

	.brandList {
		width: 48%;
		height: 250rpx;
		position: relative;
		margin-top: 10rpx;
	}

	.pic {
		width: 100%;
		height: 100%;
	}

	.nameAll {
		position: absolute;
		top: 20rpx;
		left: 20rpx;
	}

	.price {
		color: #c7c7c7;
		margin-top: 8rpx;
	}
</style>
