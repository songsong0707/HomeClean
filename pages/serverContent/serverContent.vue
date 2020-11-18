<template>
	<view>
		<!-- 主图 -->
		<view class="headimg">
			<image :src="headimg"></image>
		</view>
		<!-- 标题 -->
		<view class="servertitle">
			<text>服务介绍</text>
		</view>
		<!-- 内容 -->
		<view class="serverContent">
			<span v-html="serverContent"></span>
		</view>
		<!-- 底部按钮 -->
		<view class="bt">
			<view class="bt2" @click="takePhone">
				<image src="../../static/kefu.png"></image>
				<text>咨询客服</text>
			</view>
			<view  class="bt2" @click="toForm">
				<image src="../../static/yy.png"></image>
				<text>立即预约</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:'',
				headimg:"/static/serverbanner.jpg",
				serverContent:'暂无简介'
			}
		},
		onLoad(options) {
			let _this=this;
			_this.id=options.id;
			let id=parseInt(_this.id);
			console.log(typeof(id))
			_this.getServerDetail(id)
			
		},
		methods: {
			takePhone(){
				uni.makePhoneCall({
				 	// 手机号
				    phoneNumber: '15034671254',
					// 成功回调
					success: (res) => {	
					},
					// 失败回调
					fail: (res) => {
						console.log('调用失败!')
					}
					
				  });
			},
			toForm(){
				uni.navigateTo({
					url:"../subscribe/subscribe?id="+this.id
				})
			},
			getServerDetail(id){
				let _this=this;
				uni.request({
					url: 'https://www.dingruiqingjie.com/prod-api/basic/navserver/navlistById', //服务器端地址
					data: {
						id:id
					},
					method: 'POST',
					header: {
						'content-type': 'application/json'
					},
					success: (res) => {
						if(res.data.serverdetail.serverDetail!=''&&res.data.serverdetail.serverDetail!=null){
							_this.serverContent=res.data.serverdetail.serverDetail.replace(/\<img/gi, '<img style="width:100%;height:auto" ');
						}
						
					}
				
				});
				
			}
		}
	}
</script>

<style>
	.bt2 image{
		width: 40upx;
		height: 40upx;
		
	}
	.bt2{
		display: flex;
		flex-direction: row;
		width: 50%;
		height: 100upx;
		border-top:1upx solid #C0C0C0;
		align-items: center;
		    /*实现水平居中*/
		    justify-content: center;
		    
		    text-align: justify;
	}
	.bt{
		display: flex;
		flex-direction: row;
		position:fixed;
		bottom: 0;
		z-index: 999;
		align-items: center;
		color: #333333;
		width: 100%;
		background-color: white;
	}
	.headimg{
		width: 100%;
	}
	.headimg image{
		width: 100%;
		height: 300upx;
	}
	.servertitle{
		font-weight: 600;
		font-size: 34upx;
		text-align: center;
		margin-bottom: 30upx;
		margin-top: 20upx;
		
	}
	.servertitle text{
		text-align: center;
	}
	.serverContent{
		width: 95%;
		margin: 0 auto;
	}

	
</style>
