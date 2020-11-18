<template>
	<section class="aui-flexView">
		<section class="">
			<div class="aui-tab" data-ydui-tab>
				<ul class="tab-nav">
					<block v-for="(menuTab,index) in menuTabs" :key="index">
						<li v-bind:id="'tabNum'+index" @click="swichMenu(index)" :class="[currentTab==index ? 'tab-nav-item tab-active' : 'tab-nav-item']">
							{{menuTab.name}}
						</li>
					</block>
				</ul>
				<div class="divHeight"></div>
				<div class="tab-panel">
					<block v-for="(menuList,index2) in menuLists" :key="index2">
						<div :class="[currentTab==index2 ? 'tab-panel-item tab-active' : 'tab-panel-item']">
							<block v-for="(menuList2,index3) in menuList" :key="index3">
								<div class="tab-item">
									<a href="javascript:void(0);" class="aui-well-item aui-well-item-clear">
										<div class="aui-well-item-hd">
											<!-- <img :src="menuList2.logoimg" alt=""> -->
										</div>
										<div class="aui-well-item-bd">
											<!-- <h3>{{menuList2.dname}}</h3> -->
										</div>
										<span class="" v-if="menuList2.orderstatus==0">进行中</span>
										<span class="" v-if="menuList2.orderstatus==1">已完成</span>
									</a>
									<div class="aui-mail-product">
										<a href="javascript:;" class="aui-mail-product-item">
											<div class="aui-mail-product-item-hd">
												<img :src="menuList2.serverImg" alt="">
											</div>
											<div class="aui-mail-product-item-bd">
												<p>{{menuList2.serverName}}</p>
											</div>
										</a>
									</div>
									<div class="aui-mail-button">
										<a href="javascript:;" :class="aui-df-color" @click="takePhone">联系客服</a>
									</div>
								</div>
								<div :class="[index3+1==menuList.length ? 'hd':'divHeight']"></div>
							</block>
						</div>
					</block>
				</div>
			</div>
		</section>
	</section>
</template>

<script>
	export default {
		data() {
			return {
				scrollLeft: 0,
				isClickChange: false,
				currentTab: 0,
				menuTabs: [
					{
						name: '全部'
					}, {
						name: '进行中'
					}, {
						name: '已完成'
					}
				],
				menuLists: [],
				openid:''
			}
		},
		onLoad() {
			// for (var i = 0; i < this.menuLists.length; i++) {
			// 	this.getDateList(i);
			// }
			let openid = uni.getStorageSync("openid");
			this.openid=openid;
			this.getOrder();
		},
		 onPullDownRefresh() {
		      this.getOrder();
		    },
		onShow() {
			//验证用户是否登录
			let openid = uni.getStorageSync("openid");
			this.openid=openid;
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
			//根据Openid查询订单
			getOrder(){
				let _this=this;
				_this.menuLists=[];
				uni.request({
					url: 'https://www.dingruiqingjie.com/prod-api/basic/order/getOrderByOpenId', //服务器端地址
					data: {
						openid:_this.openid
					},
					method: 'POST',
					header: {
						'content-type': 'application/json'
					},
					success: (res) => {
						console.log(res)
						_this.menuLists.push(res.data.orderlist.allorder);
						_this.menuLists.push(res.data.orderlist.jxorder);
						_this.menuLists.push(res.data.orderlist.comorder);
					}
				
				}); 
			},
			swichMenu: async function(current) { //点击其中一个选项
				if (this.currentTab == current) {
					return false;
				} else {
					this.currentTab = current;
					this.setScrollLeft(current);
				}
			},
			swiperChange: async function(e) {
				let index = e.target.current;
				this.setScrollLeft(index);
				this.currentTab = index;
			},
			setScrollLeft: async function(tabIndex) {
				let leftWidthSum = 0;
				for (var i = 0; i <= tabIndex; i++) {
					let nowElement = await this.getWidth('tabNum' + i);
					leftWidthSum = leftWidthSum + nowElement.width;
				}
				let winWidth = uni.getSystemInfoSync().windowWidth;
				this.scrollLeft = leftWidthSum > winWidth ? (leftWidthSum - winWidth) : 0
			},
			getWidth: function(id) { //得到元素的宽高
				return new Promise((res, rej) => {
					uni.createSelectorQuery().select("#" + id).fields({
						size: true,
						scrollOffset: true
					}, (data) => {
						res(data);
					}).exec();
				})
			},
			getDateList: function(tabIndex) {
				// var entity = this.menuTabs[tabIndex].name;
				// this.menuLists[tabIndex].push(entity);
			},
		}
	}
</script>

<style>
	@import '../../css/style.css';
</style>
