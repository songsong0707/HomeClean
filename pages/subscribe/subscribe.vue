<template>
	<view>
		<textView :content="content"/>
		<inputs 
		:inputsArray="inputsArray" 
		activeName="立即预约" 
		@activeFc="activeFc" 
		:fontSizeScaleSet="fontSizeScaleSet"
		/>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				content: '请如实填写信息，以便为您提供更精准的服务',
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
						"type": "picker-date",
						"title": "预约时间"
					},
					{
						"type": "picker-city",
						"title": "预约地址",
						"variableName": "city"
					},
					{
						type: "textarea",
						title: "详细地址",
					}
				],
				serverId:''
			}
		},
		onLoad(options) {
			this.serverId=options.id;
		},
		methods: {
			//手机号验证
			 checkMobile(mobile){
			   return /^1[23456789]\d{9}$/.test(mobile);
			},
			//正则验证
			
			activeFc(res) {
				let flag1=true;
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
				if(flag1){
					uni.request({
						url: 'https://www.dingruiqingjie.com/prod-api/basic/order/addOrder', //服务器端地址
						data: {
							serverId:this.serverId,
							username:res.data_0,
							phone:res.data_1,
							address:res.city.label+res.data_4,
							time:new Date(res.data_2),
							openid:uni.getStorageSync("openid")
							
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
								uni.switchTab({
									url:'../order/order'
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
