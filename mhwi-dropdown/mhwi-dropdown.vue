<template>
	<view class="">
		<view class="mhwi-dropdown flex align-center justify-between">
			<view class="flex align-center">
				<image v-if="icon" :src="icon" mode="widthFix" style="width: 1.5vw;margin-right: 0.5vw;" />
				<text style="color: #EDEDED;">{{title}}</text>
			</view>
			<image src="@/static/image/mhwi_arrow_right.png" mode="widthFix" class="mhwi-dropdown-arrow" :class="show ? '' : 'mhwi-dropdown-arrow-disabled'"
				@click="show = !show" />
		</view>
		<view v-for="(item,index) in list" :key="index" class="mhwi-dropdown-item flex align-center" :style="{transform: show ? '' : `translateY(-${list.length + 1}00%)`}" :class="show ? '' : 'mhwi-dropdown-item-disabled'" @click="handleItemClick(item)">
			<image :src="item.icon" mode="widthFix" style="width: 3vw;margin-right: 1vw;" />
			<view class="flex flex-column justify-around">
				<text class="mhwi-dropdown-item-title line-1">{{item.title}}</text>
				<text class="mhwi-dropdown-item-info line-2">{{item.info}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	/**
	  * dropdown 下拉列表
	  * @description 提供一个下拉列表
	  * 
	  * @property {String} icon 列表头的icon
	  * @property {String} title 列表头的名称
	  * @property {Array} list 列表的子结果 Array<Object>形式
	  * @property {String} icon	子结果的icon
	  * @property {String} title 子结果的名称
	  * @property {String} info 子结果的缩略信息
	  * 
	  * @event {Function()} open 通过refs调用组件内部方法展开下拉列表
	  * @event {Function()} close 通过refs调用组件内部方法收起下拉列表
	  * @event {Function()} itemClick 子内容被点击时调用，参数为子内容
	  * @example <mhwi-dropdown ref="mhwiDropdown" :icon="icon" :title="title" :list="list" @itemClick="handleItemClick" /> 
	  * */
	export default {
		data() {
			return {
				show: true
			}
		},
		props: {
			icon: {
				type: String,
				default: ''
			},
			title: {
				type: String,
				default: ''
			},
			list: {
				type: Array,
				default: []
			}
		},
		methods: {
			open() {
				this.show = true;
			},
			close() {
				this.show = false;
			},
			handleItemClick(data) {
				this.$emit('itemClick', data);
			}
		}
	}
</script>

<style lang="scss">
	.mhwi-dropdown {
		height: 5vh;
		margin-bottom: 1vh;
	}
	.mhwi-dropdown-item {
		border-top: 5rpx solid #EDEDED;
		border-left: 5rpx solid #EDEDED;
		border-bottom: 5rpx solid #707070;
		border-right: 5rpx solid #707070;
		padding: 1vh;
		margin-bottom: 1.5vh;
		transition: all 0.5s linear;
		transform: translateY(0vh);
		opacity: 1;
		
		.mhwi-dropdown-item-title {
			color: $mhwi-accent;
			margin-bottom: 1vh;
			font-size: 2vh;
		}
		
		.mhwi-dropdown-item-info {
			color: #EDEDED;
			font-size: 1vh;
		}
	}
	
	.mhwi-dropdown-item-disabled {
		opacity: 0;
	}
	
	.mhwi-dropdown-item:hover {
		border: 5rpx solid $mhwi-border-hover;
	}
	
	.mhwi-dropdown-arrow {
		height: 2vh;
		width: 1.5vw;
		transition: all 0.25s linear;
		transform: rotate(0deg);
	}
	
	.mhwi-dropdown-arrow-disabled {
		transform: rotate(90deg);
	}
</style>
