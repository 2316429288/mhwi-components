<template>
	<view class="mhwi-mask" hover-stop-propagation :style="[transformStyle, {'z-index': zIndex}]" @click="handleMaskClick" @touchmove.stop.prevent="() => {}" :class="{
		'mhwi-mask-show': show
	}">
		<slot />
	</view>
</template>

<script>
	/**
	  * mask 遮罩层
	  * @description 提供一个全屏的遮罩层
	  * 
	  * @property {Boolean} maskClickAble 是否可以通过点击遮罩进行关闭
	  * @property {Boolean} show 是否显示遮罩
	  * @property {String} zIndex 层级z-index
	  * 
	  * @event {Function()} maskClick 遮罩层被点击
	  * @example <mhwi-mask :maskClickAble="true" :show="true" :zIndex="2" @maskClick="handleMaskClick" /> 
	  * */
	export default {
		data() {
			return {
				transformStyle: {
					transform: ''
				}
			}
		},
		props: {
			// 是否可以通过点击遮罩进行关闭
			maskClickAble: {
				type: Boolean,
				default: true
			},
			// 是否显示遮罩
			show: {
				type: Boolean,
				default: false
			},
			// 层级z-index
			zIndex: {
				type: String,
				default: '2'
			}
		},
		methods: {
			handleMaskClick() {
				if (!this.maskClickAble) return;
				this.$emit('maskClick');
			}
		},
		watch: {
			show(val) {
				if(val) {
					this.transformStyle.transform = 'scale(1, 1)';
				} else {
					this.transformStyle.transform = 'scale(1.2, 1.2)';
				}
			}
		}
	}
</script>

<style lang="scss">
	.mhwi-mask {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		opacity: 0;
		transition: all 0.3s ease-in-out;
		transform: scale(1.2, 1.2);
		background: rgba(0, 0, 0, 0.4);
	}
	
	.mhwi-mask-show {
		opacity: 1;
	}
</style>
