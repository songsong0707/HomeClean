<template>
	<view>
		<textView :content="content"/>
		<inputs 
		:inputsArray="inputsArray" 
		activeName="提交信息" 
		@activeFc="activeFc" 
		:fontSizeScaleSet="fontSizeScaleSet"
		/>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				content: '信息提交后，工作人员会在三个工作日内联系您',
				fontSizeScaleSet: { //inputs内的字体大小系数设置(字体大小为屏幕宽高度以此系数)
					allScale: .028,
					titleScale: .035,	//优先于allScale
					contentScale: .035	//优先于allScale
				},
				inputsArray: [
					{
						title: '姓名'
					},
					{
						title: '电话'
					},
					{
						"title": "身份证号"
					},
					{
						"type": "picker-city",
						"title": "地址",
						"variableName": "city"
					}
				],
			}
		},
		onShow() {
			//验证用户是否登录
			let openid = uni.getStorageSync("openid");
			let isCanUse=uni.getStorageSync("isCanUse");
			if(openid==null||openid==""||isCanUse==true){
				uni.showLoading({
					title: "请先登录"
				})
				uni.switchTab({
					url:"../center/center"
				})
			}
		},
		methods: {
			//手机号验证
			 checkMobile(mobile){
			   return /^1[23456789]\d{9}$/.test(mobile);
			},
			//身份证验证
			 IDCard(value) {
			    let reg = /^[1-9]\d{5}[1-9]\d{3}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])\d{3}([0-9]|X)$/;
				return reg.test(value)
			},
			activeFc(res) {
				let _this=this;
				let flag1=true;
				let flag2=true;
				//正则验证
				if(!this.checkMobile(res.data_1)){
					flag1=false;
					uni.showToast({
						title:"请正确填写手机号",
						icon:"none"
					});
					return;
				}else{
					flag1=true;
				}
				if(!this.IDCard(res.data_2)){
					flag2=false;
					uni.showToast({
						title:"请正确填写身份证号",
						icon:"none"
					})
				}else{
					flag2=true;
				}
				if(flag1&&flag2){
					uni.request({
						url: 'https://www.dingruiqingjie.com/prod-api/basic/cooperate/addCooperate', //服务器端地址
						data: {
							username:res.data_0,
							phone:res.data_1,
							idCode:res.data_2,
							adress:res.city.label
						},
						method: 'POST',
						header: {
							'content-type': 'application/json'
						},
						success: (res) => {
							if(res.data.code==200){
								uni.showToast({
									title: "提交成功"
								})
							}
							
						}
					
					}); 
				}
				
			}
		}
	}
</script>

<style>
</style>
