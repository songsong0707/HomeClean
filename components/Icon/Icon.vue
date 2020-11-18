<template>
	<!-- #ifdef MP-ALIPAY -->
	<text class="iconfont"  :style="{color:color,'font-size':size}" :class="iconfont"></text>
	<!-- #endif -->
	<!-- #ifndef MP-ALIPAY -->
	<rich-text class="iconfont"  v-if="type" :style="{color:color,'font-size':fontSize}" @click="onClick()" :nodes="nodes"></rich-text>
	<text v-else  class="iconfont" :style="{color:color,'font-size':fontSize}" :class="iconfont"></text>
	<!-- #endif -->
</template>

<script>
	export default {
		props: {
			/**
			 * 图标类型
			 */
			iconfont: String,
			/**
			 * 图标类型
			 */
			type: String,
			/**
			 * 图标颜色
			 */
			color: String,
			/**
			 * 图标大小
			 */
			size: {
				type: [Number, String],
				default: 32
			}
		},
		computed: {
			fontSize() {
				let  size = Number(this.size)
				size = isNaN(size) ? 32 : size * 2
				console.log(size)
				return `${size}rpx`
			},
			nodes(){
				return [{"text":this.type,"type":"text"}]
			}
		},
		methods: {
			onClick() {
				this.$emit('click')
			}
		},
	}
</script>

<style lang="scss" scoped>
// https:
	
	// 在线
	//@font-face {
	  // font-family: 'iconfont';  /* project id 1278045 */
	  // src: url('https://at.alicdn.com/t/font_1813491_w7zghhhggxq.ttf') format('truetype')
	// }
	
	// 本地
	@font-face {
	  font-family: 'iconfont';  /* project id 1278045 */
	  src: url('~@/static/icon-font.ttf') format('truetype')
	}
	.iconfont{
	font-family:"iconfont" !important;
	font-size:16px;font-style:normal;
	display: inline-block;
	-webkit-font-smoothing: antialiased;
	-webkit-text-stroke-width: 0.2px;
	-moz-osx-font-smoothing: grayscale;
	display: flex;
	align-items: center;
	}
	.iconstatus-test:before {
	  content: "\e633";
	}
</style>
