<template>
	<view>
		<view class="imageAll flex alii-center juscb">
			<view class="flex alii-center">
				<image src="../../image/a4a33f7252be84be7871b26b2397e671.jpg" mode="" class="image" v-if="face ===''"></image>
				<image :src="face" mode="" class="image" v-if="face !==''"></image>
				<!-- #ifdef MP-WEIXIN -->
				<view class="loginName fw6" @click="Login" v-if="nickname===''">
					点击登录
				</view>
				<!-- #endif -->
				<!-- #ifdef H5 || APP-PLUS-->
				<view class="loginName fw6" @click="Logins" v-if="nickname===''">
					点击登录
				</view>
				<!-- #endif -->
				<view class="loginName fw6" v-if="nickname!==''">
					{{nickname}}
				</view>
			</view>
			<image src="../../image/gos.png" mode="" v-if="nickname!==''" class="gos" @click="outlogin"></image>
		</view>
		<u-modal v-model="show" :content="content" @confirm='sureLogin' :show-cancel-button='true'></u-modal>
		<u-modal v-model="shows" :content="contents" @confirm='sureOut' :show-cancel-button='true'></u-modal>
		<view class="davAll flex fwrap ">
			<view class="davList flex column jus-center alii-center" :class="(index+1)%3 == 0?'border':''" v-for="(item,index) in davList"
			 :key="item.id" @click="goto(item)">
				<image :src="item.icon" mode="" class="icon"></image>
				<view class="">
					{{item.name}}
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
				davList: [{
						name: '我的订单',
						icon: require('../../image/dingdan.png')
					},
					{
						name: '优惠券',
						icon: require('../../image/youhui.png')
					},
					{
						name: '礼品卡',
						icon: require('../../image/lipin.png')
					},
					{
						name: '我的收藏',
						icon: require('../../image/shoucang.png')
					},
					{
						name: '我的足迹',
						icon: require('../../image/zuji.png')
					},
					{
						name: '会员福利',
						icon: require('../../image/vip.png')
					},
					{
						name: '地址管理',
						icon: require('../../image/dizhi.png')
					},
					{
						name: '账号安全',
						icon: require('../../image/anquan.png')
					},
					{
						name: '联系客服',
						icon: require('../../image/kefu.png')
					},
					{
						name: '帮助中心',
						icon: require('../../image/bangzhu.png')
					},
					{
						name: '意见反馈',
						icon: require('../../image/yijian.png')
					},
				],
				show: false,
				shows: false,
				content: '即将使用微信账号登录，是否同意',
				contents: '是否退出登录',
				face: '',
				nickname: '',
				// faces: '',
				// nicknames: '',
			}
		},
		methods: {
			goto(item) {
				if (item.name === '我的收藏') {
					uni.navigateTo({
						url: '../collect/collect'
					})
				}
				if (item.name === '地址管理') {
					uni.navigateTo({
						url: '../addressList/addressList'
					})
				}
			},

			//微信登录
			WeChatUp() {
				uni.getProvider({
					service: 'oauth',
					success: (res) => {
						console.log(res.provider)
						if (~res.provider.indexOf('weixin')) {
							uni.login({
								provider: 'weixin',
								success: (loginRes) => {
									console.log(loginRes);
									// 获取用户信息
									uni.getUserInfo({
										provider: 'weixin',
										success: (infoRes) => {
											console.log('用户昵称为：' + infoRes.userInfo.nickName);
											console.log(infoRes);
											this.face = infoRes.userInfo.avatarUrl;
											this.nickname = infoRes.userInfo.nickName;
										}
									});
								}
							});
						}
					}
				});
			},
			Login() {
				// 授权
				uni.authorize({
					scope: 'scope.userInfo',
					success() {
						this.WeChatUp()
					}
				})
			},
			Logins() {
				this.show = true;
			},
			sureLogin() {
				this.WeChatUp()
			},
			outlogin(){
				this.shows = true;
			},
			sureOut() {
				// uni.navigateTo({
				// 	url: '../outLogin/outLogin'
				// })
				this.face=''
				this.nickname =''
			},
		},
		mounted() {

		},
		onLoad(options) {
		console.log(options);
			if (options.face === '0') {
				this.faces = 0
			}
			if (options.nickname === '0') {
				this.nicknames = 0
			}
		},
		onShow() {
			console.log(this.faces);
			if (this.faces ===0) {
				this.face = ''
			}
			if (this.nicknames === 0) {
				this.nickname = ''
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
	.imageAll {
		height: 300rpx;
		background: rgb(180, 40, 45);
		padding: 0 30rpx;
	}

	.image {
		width: 200rpx;
		height: 200rpx;
		border-radius: 50%;
		// border: 1rpx solid #999;
	}

	.gos {
		width: 70rpx;
		height: 70rpx;
	}

	.loginName {
		color: #fff;
		font-size: 38rpx;
		margin-left: 30rpx;
	}

	.icon {
		width: 60rpx;
		height: 60rpx;
		margin-bottom: 20rpx;
	}

	.davList {
		width: 33%;
		height: 200rpx;
		border-top: 1rpx solid #c9c9c9;
		border-right: 1rpx solid #c9c9c9;
	}

	.davAll {
		padding: 0 20rpx;
	}

	.border {
		border-right: none;
	}
</style>
