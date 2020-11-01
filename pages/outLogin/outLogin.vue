<template>
    <view>
			<u-button type="error" @click='outLogin'>退出登录</u-button>
	
    </view>
</template>

<script>
export default {
name: "",
components: {

},
props: {},
data () {
  return {
    }
  },
  methods: {
	outLogin(){
		uni.switchTab({
			url:`../my/my?face=0&nickname=0`
		})
	}
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

</style>
