<template>
	<view class="dd-progress-box">
		<view v-if="displayTitle" class="dd-progress-left-title" v-text="leftValue"></view>
		<view class="dd-progress-value-box" :style="{height: height + 'rpx', borderRadius: borderRadius + 'rpx', backgroundColor: backgroundColor}">
		  <view class="dd-progress-left" :style="{width: leftValue / 2 + '%',backgroundColor: leftBackgroundColor ,borderBottomLeftRadius: borderRadius + 'rpx', borderTopLeftRadius: borderRadius + 'rpx',}"></view>
			<view class="dd-progress-mind-line"></view>
			<view class="dd-progress-right" :style="{width: rightValue / 2 + '%',backgroundColor: rightBackgroundColor ,borderBottomRightRadius: borderRadius + 'rpx', borderTopRightRadius: borderRadius + 'rpx',}"></view>
		</view>
		<view v-if="displayTitle" class="dd-progress-right-title" v-text="rightValue"></view>
	</view>
</template>

<script>
/**  
* 组件名称
* @description 组件注释  
* @tutorial 文档地址  
* @property {属性值的类型} 属性名 属性描述
* @event {Function} 事件名 事件描述  
* @example <dd-xxx></dd-xxx>  
*/	
export default {
  name: "DdTrendProgress",
  props: {
	  height: {
	  	type: [Number, String],
	  	default: 8
	  },
	  leftValue: {
	  	type: Number,
	  	default: 0
	  },
	  rightValue: {
	  	type: Number,
	  	default: 0
	  },
	  displayTitle: {
	  	type: Boolean,
	  	default: true
	  },
	  borderRadius: {
	  	type: [Number, String],
	  	default: 4
	  },
	  backgroundColor: {
	  	type: String,
	  	default: '#EBECF0'
	  },
	  leftBackgroundColor: {
	  	type: String,
	  	default: '#E27373'
	  },
	  rightBackgroundColor: {
	  	type: String,
	  	default: '#37C863'
	  },
  },
  data() {
    return {
		
    };
  },
  methods:{
	  
  }
};
</script>

<style lang="scss" scoped>
	.dd-progress-box{
		display: flex;
		align-items: center;
		.dd-progress-left-title{
			margin-right: 8rpx;
			font-size: 28rpx;
			color: #111111;
			@include fontFamilyLight();
		}
		.dd-progress-value-box{
			position: relative;
			display: flex;
			align-items: center;
			justify-content: center;
			flex: 1;
			.dd-progress-left{
				position: absolute;
				height: 100%;
				right: 50%;
			}
			.dd-progress-mind-line{
				position: absolute;
				width: 2rpx;
				height: 12rpx;
				background-color: #ABAEBA;
				z-index: 3;
			}
			.dd-progress-right{
				position: absolute;
				height: 100%;
				left: 50%;
			}
		}
		.dd-progress-right-title{
			margin-left: 8rpx;
			font-size: 28rpx;
			color: #111111;
			@include fontFamilyLight();
		}
	}
</style>

