<template>
	<view>
		index
	</view>
</template>

<script>
	export default {
		name: "",
		components: {

		},
		props: {},
		data() {
			return {}
		},
		methods: {

		},
		mounted() {
			uni.chooseLocation({
				success: function(res) {
					console.log('位置名称：' + res.name);
					console.log('详细地址：' + res.address);
					console.log('纬度：' + res.latitude);
					console.log('经度：' + res.longitude);
					uni.switchTab({
						url:`../home/home?address=${res.name}`
					})
				}
			});
			// uni.getLocation({
			//     type: 'gcj02', //返回可以用于uni.openLocation的经纬度
			//     success: function (res) {
			// 		console.log(res);
			//         const latitude = res.latitude;
			//         const longitude = res.longitude;
			//         uni.openLocation({
			//             latitude: latitude,
			//             longitude: longitude,
			//             success: function () {
			//                 console.log('success');
			//             }
			//         });
			//     }
			// });
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

</style>
