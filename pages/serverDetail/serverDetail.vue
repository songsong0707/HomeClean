<template>
	<view>
		<view v-for="(item,index) in serverList" :key="index" class="serverdetail">
			<!-- 分类标题 -->
			<view class="title_type">
				<!-- 色块 -->
				<view class="title_color"></view>
				<text>{{item.typeName}}</text>
			</view>
			<!-- 分类宫格 -->
			<view>
				<ju-navigator-grid element-id="navigatorMenu" :list="item.listnavserver" @press="onPress" height="180" size="90" />
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				serverList: []
			}
		},
		onLoad() {
			this.getAllServer()
		},
		methods: {
			getAllServer(){
				let _this=this;
				uni.request({
					url: 'https://www.dingruiqingjie.com/prod-api/basic/navserver/getAllServer', //服务器端地址
					data: {
					},
					method: 'GET',
					header: {
						'content-type': 'application/json'
					},
					success: (res) => {
						console.log(res);
						_this.serverList=res.data.serverList;
					}
				
				});
			}
		}
	}
</script>

<style>
	.title_type {
		display: flex;
		flex-direction: row;
		font-size: 30upx;
	}

	.title_color {
		width: 10upx;
		height: 40upx;
		background-color: #4CD964;
		margin-right: 10upx;

	}

	.serverdetail {
		width: 95%;
		margin: 0 auto;
	}
</style>
