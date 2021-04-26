<template>
	<view class="dd-search-box" :style="{margin: marginInfo}">
		<view class="dd-search-icon" :class="{right: iconPosition != 'left', left: iconPosition == 'left'}">
			<dd-icon name="Diansousuo-bai" marginInfo="0 5px 0 0" size="30" color="#ABAEBA"></dd-icon>
		</view>
		<u-input v-model="currentValue" type="text" :clearable="clearable" :placeholder="placeholder" :placeholderStyle="placeholderStyle" class="dd-input" :class="{'icon-right': iconPosition != 'left'}" :style="{backgroundColor: backgroundColor, borderRadius:borderRadius + 'px'}" :custom-style="{borderRadius:borderRadius + 'px', padding: iconPosition == 'left' ? '0 35px 0 36px' : '0 60px 0 15px'}" />
	</view>
</template>

<script>
	import DdIcon from "@/components/base/icon/DdIcon.vue";
	/**  
	 * 组件名称
	 * @description 组件注释  
	 * @tutorial 文档地址  
	 * @property {属性值的类型} 属性名 属性描述
	 * @event {Function} 事件名 事件描述  
	 * @example <dd-xxx></dd-xxx>  
	 */
	export default {
		name: "DdSearch",
		props: {
			value: {
				type: String,
				default: ''
			},
			clearable: {
				type: Boolean,
				default: true
			},
			placeholder: {
				type: String,
				default: ''
			},
			placeholderStyle: {
				type: String,
				default: ''
			},
			iconPosition: {
				type: String,
				default: 'left'
			},
			borderRadius: {
				type: [Number, String],
				default: 24
			},
			backgroundColor: {
				type: String,
				default: '#EBECF0'
			},
			marginInfo: {
				type: String,
				default: '15px'
			},
		},
		components: { DdIcon },
		computed: {
			currentValue: {
				get: function() {
					return this.value;
				},
				set: function(val) {
					this.$emit("input", val);
				}
			}
		},
		data() {
			return {

			};
		},
		methods: {

		}
	};
</script>

<style lang="scss" scoped>
.dd-search-box{
	position: relative;
	display: flex;
	align-items: center;
	.dd-search-icon{
		position: absolute;
		// left: 10px;
		z-index: 100;
	}
	.dd-search-icon.left{
		left: 10px;
	}
	.dd-search-icon.right{
		right: 10px;
	}
	.dd-input{
		display: flex;
		align-items: center;
		/deep/ .u-input__right-icon{
			position: absolute;
			right: 15px;
		}
	}
	.dd-input.icon-right{
		/deep/ .u-input__right-icon{
			position: absolute;
			right: 40px;
		}
	}
}
</style>
