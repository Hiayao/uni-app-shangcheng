<template>
	<view class="All">
		<view>
			<u-form :model="form" ref="uForm">
				<u-form-item label="" prop="name">
					<u-input v-model='form.name' @input='iptName' placeholder='姓名' />
				</u-form-item>
				<u-form-item label="" prop="tel">
					<u-input v-model="form.tel" type='number' @input='iptTel' placeholder='手机号码' />
				</u-form-item>
				<u-form-item label="" prop="city">
					<u-input v-model='form.city' :disabled='true' placeholder='省份/城市/区县' @click='choice' />
				</u-form-item>
				<u-form-item label="" prop="address">
					<u-input v-model='form.address' @input='iptRess' placeholder='详细地址 , 如楼盘、单元等' />
				</u-form-item>
			</u-form>
		</view>
		<!-- 城市选择器 -->
		<u-picker mode="region" v-model="show" :area-code='["51", "5101", "510112"]' @confirm='sure'></u-picker>

		<view class="moren flex juscb alii-center">
			<u-checkbox @change="checkboxChange" v-model="checked">设置默认地址</u-checkbox>
			<view class="inwx">
				一键导入微信 ＞
			</view>
		</view>

		<view class="btn">
			<u-button type="error" @click='gotoAdd'>保存</u-button>
		</view>
		<view class="btn">
			<u-button type="error" v-if="id !==''" @click='modify'>修改</u-button>
		</view>
	</view>
</template>

<script>
	import sempCity from "@/components/semp-city/semp-city.vue"
	export default {
		name: "",
		components: {
			sempCity
		},
		props: {},
		data() {
			return {
				id: '', //地址的id
				show: false,
				checked: false,
				city: '',
				form: {
					name: '',
					tel: '',
					city: '',
					address: '',
				},
				rules: {
					name: [{
						required: true,
						message: '请输入姓名',
						// 可以单个或者同时写两个触发验证方式 
						trigger: ['change', 'blur'],
					}],
					tel: [{
							required: true,
							message: '请输入手机号码',
							trigger: ['change', 'blur'],
						},
						{
							min: 11,
							max: 11,
							message: '手机号码为11位',
							trigger: ['change', 'blur'],
						},
						{
							// 自定义验证函数，见上说明
							validator: (rule, value, callback) => {
								// 上面有说，返回true表示校验通过，返回false表示不通过
								// this.$u.test.mobile()就是返回true或者false的
								return this.$u.test.mobile(value);
							},
							message: '不是中国手机号码',
							// 触发器可以同时用blur和change
							trigger: ['change', 'blur'],
						}
					],
					city: [{
						required: true,
						message: '请选择城市',
						trigger: ['change', 'input'],
					}],
					address: [{
						required: true,
						message: '请输入详细地址',
						trigger: ['change', 'blur'],
					}]
				}
			}
		},
		methods: {
			gotoAdd() {
				this.$refs.uForm.validate(valid => {
					console.log(valid);
					if (valid) {
						console.log('验证通过');
						uni.request({
							// #ifdef H5
							url: `/api/address/saveAction`,
							// #endif
							// #ifdef MP-WEIXIN || APP-PLUS 
							url: `${this.$api}/address/saveAction`,
							// #endif
							method: 'POST',
							data: {
								address: this.form.city,
								checked: this.checked,
								detailadress: this.form.address,
								openId: "oRrdQt3Tkr7_DfmIAlt9odmhH8kQ",
								telNumber: this.form.tel,
								userName: this.form.name
							},
							success: res => {
								console.log(res);
								uni.navigateTo({
									url: '../addressList/addressList'
								})
							},
							fail: () => {},
							complete: () => {}
						});
					} else {
						console.log('验证失败');
					}
				});
			},
			// 点击选择城市弹出层
			choice() {
				this.show = true
			},
			// 确认选择城市
			sure(e) {
				console.log(e);
				// this.city = e.province.label + e.city.label + e.area.label
				this.form.city = e.province.label + '/' + e.city.label + '/' + e.area.label
				console.log(this.form.city);
			},
			// 输入姓名
			iptName(e) {
				this.form.name = e
			},
			// 输入电话
			iptTel(e) {
				this.form.tel = e
			},
			// 输入详细地址
			iptRess(e) {
				this.form.address = e
			},
			// 选择是否默认地址
			checkboxChange(e) {
				console.log(e);
				this.checked = e.value
			},
			// 修改地址
			modify() {
				this.$refs.uForm.validate(valid => {
					console.log(valid);
					if (valid) {
						console.log('验证通过');
						uni.request({
							// #ifdef H5
							url: `/api/address/saveAction`,
							// #endif
							// #ifdef MP-WEIXIN || APP-PLUS 
							url: `${this.$api}/address/saveAction`,
							// #endif
							method: 'POST',
							data: {
								address: this.form.city,
								addressId: this.id,
								checked: this.checked,
								detailadress: this.form.address,
								openId: "oRrdQt3Tkr7_DfmIAlt9odmhH8kQ",
								telNumber: this.form.tel,
								userName: this.form.name
							},
							success: res => {
								console.log(res);
								uni.navigateTo({
									url: '../addressList/addressList'
								})
							},
							fail: () => {},
							complete: () => {}
						});
					} else {
						console.log('验证失败');
					}
				});
			},
		},
		mounted() {

		},
		onLoad(options) {
			// console.log(options);
			// 判断是修来点进来的还是新增点进来的
			if(options.id){
			console.log(JSON.parse(options.list));
			let list = JSON.parse(options.list)
			if (list) {
				this.form.name = list.name
				this.form.tel = list.mobile
				this.id = list.id
				this.form.city = list.address
				this.form.address = list.address_detail
				if (list.is_default === 1) {
					this.checked = true
				} else {
					this.checked = false
				}
			}
			}
		},
		// 必须要在onReady生命周期，因为onLoad生命周期组件可能尚未创建完毕
		onReady() {
			this.$refs.uForm.setRules(this.rules);
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
		padding: 0 20rpx;
	}

	.btn {
		position: fixed;
		bottom: 0;
		width: 80%;
		margin-left: 10%;
	}

	.inwx {
		color: rgb(55, 155, 55);
	}

	.moren {
		height: 100rpx;
	}
</style>
