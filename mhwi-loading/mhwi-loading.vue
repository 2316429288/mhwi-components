<template>
	<view v-if="visibleSync">
		<view class="mhwi-loading flex align-center justify-center" :class="show ? 'mhwi-loading-show' : ''">
			<text class="iconfont mhwi-loading-icon" :style="{color: loadingColor}" />
		</view>
	</view>
</template>

<script>
	/**
	  * loading 加载动画
	  * @description 提供一个加载动画
	  * 
	  * @property {String} loadingColor 加载动画的颜色
	  * 
	  * @event {Function()} open 通过refs调用组件内部方法显示加载动画
	  * @event {Function()} close 通过refs调用组件内部方法关闭加载动画
	  * @example <mhwi-loading ref="mhwiLoading" loadingColor="#ededed" /> 
	  * */
	export default {
		data() {
			return {
				visibleSync: false,
				show: false
			}
		},
		methods: {
			open() {
				this.change('visibleSync', 'show', true);
			},
			close() {
				this.change('show', 'visibleSync', false);
			},
			// 此处的原理是，关闭时先通过动画隐藏弹窗和遮罩，再移除整个组件
			// 打开时，先渲染组件，延时一定时间再让遮罩和弹窗的动画起作用
			change(param1, param2, status) {
				this[param1] = status;
				if (status) {
					// #ifdef H5 || MP
					this.timer = setTimeout(() => {
						this[param2] = status;
						this.$emit(status ? 'open' : 'close');
					}, 100);
					// #endif
					// #ifndef H5 || MP
					this.$nextTick(() => {
						this[param2] = status;
						this.$emit(status ? 'open' : 'close');
					})
					// #endif
				} else {
					this.timer = setTimeout(() => {
						this[param2] = status;
						this.$emit(status ? 'open' : 'close');
					}, 500);
				}
			}
		},
		props: {
			loadingColor: {
				type: String,
				default: '#EDEDED'
			}
		}
	}
</script>

<style>
	.mhwi-loading {
		transition: all 0.5s;
		transform: translateY(-10vh);
	}

	.mhwi-loading-show {
		transform: translateY(0vh);
	}

	.mhwi-loading-icon::before {
		display: inline-block;
		vertical-align: middle;
		animation: loadingRotate 1s linear infinite;
		transform-origin: center center;
		content: '\e926';
		font-size: 3vh;
	}

	@keyframes loadingRotate {
		0% {
			transform: rotate(0deg);
		}

		100% {
			transform: rotate(-360deg);
		}
	}
</style>
