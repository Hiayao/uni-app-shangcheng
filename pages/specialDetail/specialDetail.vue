<template>
	<view class="All">
		<u-parse :html="content"></u-parse>
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
				content:''
			}
		},
		methods: {

		},
		mounted() {

		},
		onLoad(options) {
			console.log(options);
			uni.request({
				url: `/api/topic/detailaction?id=${options.id}`,
				method: 'GET',
				data: {},
				success: res => {
					console.log(res);
					this.content = res.data.data.content
				},
				fail: () => {},
				complete: () => {}
			});
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
.All{
	font-size: 0;
}
</style>
