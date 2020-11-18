<template>
	<view>
		<!-- 主图 -->
		<view class="headimg">
			<image :src="headimg"></image>
		</view>
		<!-- 标题 -->
		<view class="title">
			<text>{{newsTitle}}</text>
		</view>
		<!-- 内容 -->
		<view class="content">
			<view v-html="newsContent"></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				headimg:'',
				newsTitle:'',
				newsContent:'',
				id:''
			}
		},
		onLoad(options) {
			this.id=options.id;
			this.getNewsDetail();
		},
		methods: {
			getNewsDetail(){
				let _this=this;
				uni.request({
					url: 'https://www.dingruiqingjie.com/prod-api/basic/news/getNewsDetail', //服务器端地址
					data: {
						id:_this.id
					},
					method: 'POST',
					header: {
						'content-type': 'application/json'
					},
					success: (res) => {
						_this.newsTitle=res.data.data.newsTitle;
						_this.headimg=res.data.data.headimg;
						_this.newsContent=res.data.data.newsContent.replace(/\<img/gi, '<img style="width:100%;height:auto" ');
					}
				
				});
			}
		}
	}
</script>

<style>
.headimg{
	width: 100%;
	height: 500upx;
}
.headimg image{
	width: 100%;
	height: 100%;
}
.title{
	width: 95%;
	margin: 0 auto;
	text-align: center;
	font-size: 36upx;
	font-weight: 700;
	margin-top: 20upx;
	margin-bottom: 20upx;
}
.content{
	width: 90%;
	margin: 0 auto;
	font-size: 28upx;
	color: #3C3C3C;
}
</style>
