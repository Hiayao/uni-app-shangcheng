<template>
    <view>
     <view class="wrap">
     		<u-swiper :list="banner" name="image_url"></u-swiper>
     	</view>
    </view>
</template>

<script>
export default {
name: "",
components: {

},
props: {
	banner:{
		type:Array
	}
},
data () {
  return {
    }
  },
  methods: {

  },
  mounted () {

  },
  onLoad () {

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
.wrap {
		padding: 10rpx;
	}
</style>
