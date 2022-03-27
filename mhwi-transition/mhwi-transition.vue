<template>
	<view>
			<uni-transition :mode-class="mode" :show="show">
				<slot></slot>
			</uni-transition>
		</view>
	</view>
</template>

<script>
	/**
	  * transition 过渡效果
	  * @description 提供一个数据的过渡效果
	  * 
	  * @property {String} mode 过渡效果 fade/slide-top/slide-right/slide-bottom/
	  * slide-left/zoom-in/zoom-out
	  * @property {Boolean} show 是否显示
	  * 
	  * @example <mhwi-transition :show="true" mode="fade" /> 
	  * */
	export default {
		data() {
			return {

			}
		},
		props: {
			mode: {
				type: String,
				default: 'fade'
			},
			show: {
				type: Boolean,
				default: false
			},
		},
		methods: {
		}
	}
</script>

<style lang="scss">

</style>
