<template>
	<view class="mhwi-toast flex" :class="show ? 'mhwi-toast-active' : ''">
		<text class="iconfont mhwi-toast-icon" :class="`mhwi-toast-icon-${type}`" />
		<text class="line-2">{{title}}</text>
	</view>
</template>

<script>
	export default {
		name: 'mhwi-toast',
		data() {
			return {
				show: false
			}
		},
		props: {
			type: {
				type: String,
				default: 'none'
			},
			title: {
				type: String,
				default: ''
			}
		},
		methods: {
			open() {
				this.show = true;
			},
			close() {
				this.show = false;
			}
		}
	}
</script>

<style lang="scss">
	.mhwi-toast {
		width: 10vw;
		height: 2vh;
		padding: 0.5vh;
		position: fixed;
		top: 5vh;
		right: -10.5vw;
		background: $mhwi-primary;
		color: $mhwi-info;
		transition: all 0.5s linear;
	}
	.mhwi-toast-active {
		right: 0vw;
	}
	.mhwi-toast-icon {
		font-size: 2vw;
	}
	.mhwi-toast-icon-none::before {
		content: '';
	}
	.mhwi-toast-icon-success::before {
		content: '\e92b';
	}
	.mhwi-toast-icon-fail::before {
		content: '\e98d';
	}
	.mhwi-toast-icon-error::before {
		content: '\e96e';
	}
</style>
