<template>
  <view class="card-box" :style="{margin: marginInfo, borderRadius: borderRadius + 'px'}">
    <view class="card-header" :class="{'header-line': headerLine}">
		<view class="card-title" v-text="title"></view>
		<slot name="title-right"></slot>
	</view>
	<view class="card-body">
		123222
	</view>
  </view>
</template>

<script>
/**  
* 卡片组件
* @description 卡片组件  
* @tutorial 文档地址  
* @property {String} title 标题名称
* @property {String} marginInfo 外边距
* @property {Boolean} headerLine 是否显示标题栏底部横线
* @property {Number, String} borderRadius 卡片圆角
* @event {Function} 事件名 事件描述  
* @example <dd-card title="卡片" :headerLine="true"></dd-card>  
*/	
export default {
  name: "DdCard",
  props: {
	  title: { type: String, default: '' },
	  marginInfo: { type: String, default: '15px' },
	  headerLine: { type: Boolean, default: false },
	  borderRadius: { type: [Number, String], default: 8 },
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
	.card-box{
		background-color: #fff;
		.card-header{
			position: relative;
			display: flex;
			padding: 12px;
			align-items: center;
			justify-content: space-between;
			.card-title{
				font-size: 16px;
				font-family: PingFangSC-Medium, PingFang SC;
				font-weight: 500;
				color: #111111;
			}
		}
		.card-header.header-line{
			border-bottom: 1px solid $u-border-color;
		}
		.card-header::before{
			@include listTitle();
		}
		.card-body{
			padding: 12px;
			font-size: 14px;
		}
	}
</style>

