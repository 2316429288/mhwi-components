<template>
	<view class="mhwi-bread-crumbs flex align-center" :style="{background: background, color: color}">
		<view v-for="(item, index) in list.list" :key="index">
			<text class="mhwi-bread-crumbs-text" :style="{color: item.active == item.title ? activeColor : ''}" @click="handleClick(item.route)">{{item.title}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		props: {
			background: {
				type: String,
				default: '#EDEDED'
			},
			color: {
				type: String,
				default: '#0b263d'
			},
			activeColor: {
				type: String,
				default: '#909090'
			}
			list: {
				type: Object,
				default: {}
			}
		},
		methods: {
			handleClick(route) {
				
			}
		}
	}
</script>

<style lang="scss">
	.mhwi-bread-crumbs {
		padding-left: 1vw;
	}
	.mhwi-bread-crumbs-text {
		
	}
	.mhwi-bread-crumbs-text::after {
		content: '/';
		margin: 0vh 1vw;
	}
</style>
