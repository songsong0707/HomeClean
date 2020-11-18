<template>
	<view class="content">
		<!-- 轮播图组件 -->
		<view>
			<swiper class="swiper" indicator-dots="true" autoplay="true" interval="5000" duration="1500">
				<swiper-item v-for="(item , index) in bannerList" :key="index">
					<image :src="item.homeImgSrc"></image>
				</swiper-item>
			</swiper>
		</view>
		<!-- 宫格组件 -->
		<view class="nine_nav">
			<ju-navigator-grid element-id="navigatorMenu" :list="menu" @press="onPress" height="180" size="90" />
		</view>
		<!-- 通知组件 -->
		<view class="notice">
			<wyb-noticeBar :text="noticeList" type="vert" bgColor="white"/>
		</view>
		<!-- 新闻组件 -->
		<view class="news" v-for="(item,index) in newsList" :key="index">
			<!-- 分类 -->
			<view class="type">
				<!-- 色块 -->
				<view class="type_color"></view>
				<!-- 分类名称 -->
				<text>{{item.typeName}}</text>
			</view>
			<!-- 新闻内容 -->
			
			<view v-for="item2 in item.newsList" :key="item2.id" class="news_content" @click="toNewsDetail(item2)">
				<!--主图 -->
				<view class="headimg">
					<image :src="item2.headimg"></image>
				</view>
				<!-- 右侧内容 -->
				<view>
					<!-- 标题 -->
					<view class="news_title">
						<text>{{item2.newsTitle}}</text>
					</view>
					<!-- 时间 -->
					<view class="news_bottom">
						<view class="news_icon">
							<image src="../../static/time.png"></image>
							<text>{{item2.createTime | formatDate}}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import wybNoticeBar from '@/components/wyb-noticeBar/wyb-noticeBar.vue'
  let products = require('@/static/json/myjson.json'); 
  let products1 = require('@/static/json/lunbo.json'); 
  var xinwen=require("@/static/json/xinwen.json")
	export default {
		 components: {
		        wybNoticeBar
		    },
		data() {
			return {
				//新闻数据
				newsList:[],
				//轮播图数据
				bannerList: [],
				//宫格导航数据
				menu: [{
						title: "",
						url: '',
						icon: ""
					},
					{
						title: "",
						url: '',
						icon: ""
					},
					{
						title: "",
						url: '',
						icon: ""
					},
					{
						title: "",
						url: '',
						icon: ""
					},
					{
						title: "",
						url: '',
						icon: ""
					},
					{
						title: "",
						url: '',
						icon: ""
					},
					{
						title: "",
						url: '',
						icon: ""
					},
					{
						title: "全部",
						url: '/pages/serverDetail/serverDetail',
						icon: "/static/全部 (1).png"
					},

				],
				//通知栏数据
				noticeList:[],
				navlist:[]
			}
		},
		onLoad() {
			this.getBannerList();
			this.getNavList();
			this.getNewsList();
			this.getNoticeList();
		},
		filters:{
					formatDate(data){
						const nDate=new Date(data);
						const year = nDate.getFullYear().toString().padStart(2,0);
						const month = nDate.getMonth().toString().padStart(2,0);
						const day = nDate.getDay().toString().padStart(2,0);
						return year +'-'+ month +'-'+ day
					}
				},
		methods: {
			onShareAppMessage: function(e) {
							let title = '鼎瑞清洁小程序'
							return {
								title: title,
								path: 'pages/cart/exchangeSc'
							}
						},
			/* 跳转资讯详情页 */
			toNewsDetail(item){
				 uni.navigateTo({
					url:"../newsDetail/newsDetail?id="+item.id
				}) 
			},
			/* 获取轮播图 */
			getBannerList(){
				let _this=this;
				_this.bannerList=products1
			},
			/* 获取导航 */
			getNavList(){
				let _this=this;
		        _this.navlist =	products
				_this.navlist.forEach((item,index)=>{
					_this.menu[index].icon=item.serverImg;
					_this.menu[index].title=item.serverName;
					_this.menu[index].url="/pages/serverContent/serverContent?"+"id="+item.id;
				})
			},
			/* 获取新闻 */
			getNewsList(){
				let _this=this;
				_this.newsList=xinwen;
						
			
			},
			/* 获取通知 */
			getNoticeList(){
				let _this=this;
				uni.request({
					url: 'https://www.dingruiqingjie.com/prod-api/basic/notice/getNoticeList', //服务器端地址
					data: {
					},
					method: 'GET',
					header: {
						'content-type': 'application/json'
					},
					success: (res) => {
						let notice=res.data.data;
						notice.forEach((item,index)=>{
							_this.noticeList.push(item.content)
						})
					}
				
				});
			}
		}
	}
</script>

<style>
	.news_title{
		font-size: 30upx;
		color: #464646;
		height: 70%;
		width: 100%;
		overflow: hidden;
		text-overflow: ellipsis;
	}
	.news_bottom{
		display: flex;
		flex-direction: row;
		font-size: 24upx;
		color:#999999 ;	
	}
	.headimg{
		margin-right: 20upx;
	}
	.news_content{
		margin-top: 30upx;
		display: flex;
		flex-direction: row;
	}
	.type{
		display: flex;
		flex-direction: row;
		text-align: center;
		align-items: center;
		font-weight: 700;
		font-size: 34upx;
	}
	.headimg image{
		height: 180upx;
		width: 240upx;
	}
	.news{
		width: 90%;
		margin: 0 auto;
		margin-top: 40upx;
	}
	.news_icon image{
		width: 24upx;
		height: 24upx;
		margin-right: 5upx;
	}
	.news_icon{
		display: flex;
		flex-direction: row;
		align-items: center;
		margin-right: 20upx;
	}
	.swiper {
		height: 400upx;
	}

	// swiper-item 里面的图片高度
	swiper-item image {
		width: 100%;
		height: 400upx;
	}

	.nine_nav {
		margin-top: 20upx;
	}
	.notice{
		margin-top: 20upx;
	}
	.type_color{
		width: 10upx;
		height: 40upx;
		background-color: #4CD964;
		margin-right: 10upx;
	}
</style>
