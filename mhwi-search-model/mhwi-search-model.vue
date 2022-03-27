<template>
	<view v-if="visibleSync" hover-stop-propagation>
		<mhwi-mask :show="maskShow" @maskClick="close" />
		<view class="mhwi-search-model flex flex-column align-center" :class="maskShow ? 'mhwi-search-model-show' : ''">
			<view class="mhwi-search-bar flex align-center justify-around">
				<text class="iconfont mhwi-search-icon" />
				<input class="mhwi-search-input flex-10" type="text" v-model="searchInput" @input="handleSearch"
					hover-stop-propagation />
				<text v-if="loading" class="iconfont mhwi-search-loading-icon" />
			</view>
			<scroll-view scroll-y :style="{height: searchData.length ? '45vh' : '0vh'}"
				style="transition: height 0.25s linear;">
				<view class="mhwi-search-data">
					<view v-for="(item, index) in searchData" :key="index">
						<mhwi-dropdown :icon="item.icon" :title="item.content" :list="item.data" @itemClick="handleItemClick" />
					</view>
				</view>
			</scroll-view>
			<view class="mhwi-search-bottom flex align-center justify-between">
				<view class="mhwi-search-slot flex-10">
					<slot>
						<text style="color: #FFFF00;">Power By Lit</text>
					</slot>
				</view>
				<view class="mhwi-search-btn" @click="close">
					<text>关闭</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	/**
	  * search-model 搜索模态框
	  * @description 提供一个弹出的搜索模态框
	  * 
	  * @property {Array} searchData 搜索结果列表 Array<Object>形式
	  * @property {String} icon 列表的icon
	  * @property {String} content 列表的分类
	  * @property {Array} data 列表的子结果 Array<Object>形式
	  * 
	  * @event {Function()} open 通过refs调用组件内部方法显示模态框
	  * @event {Function()} close 通过refs调用组件内部方法关闭模态框
	  * @event {Function()} search 用户输入搜索内容时调用，参数为用户输入的内容
	  * @event {Function()} itemClick 搜索内容子内容被点击时调用，参数为子内容
	  * @example <mhwi-search-model ref="mhwiSearchModel" :searchData="searchData" @search="handleSearch" @itemClick="handleItemClick" /> 
	  * */
	export default {
		data() {
			return {
				visibleSync: false,
				maskShow: false,
				resultShow: false,
				timer: null,
				scrollHeight: 0,
				searchInput: ''
			}
		},
		props: {
			searchData: {
				type: Array,
				default: []
			},
			loading: {
				type: Boolean,
				default: false
			}
		},
		methods: {
			open() {
				this.change('visibleSync', 'maskShow', true);
			},
			close() {
				this.searchInput = '';
				this.change('maskShow', 'visibleSync', false);
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
			},
			handleSearch() {
				this.$emit('search', this.searchInput);
			},
			handleItemClick(data) {
				this.$emit('itemClick', data);
			}
		}
	}
</script>

<style lang="scss">
	.mhwi-search-model {
		position: fixed;
		right: 0;
		left: 0;
		margin: 12vh auto 0;
		z-index: 99;
		width: 50vw;
		padding: 3vh;
		border-radius: 20rpx;
		background: $mhwi-primary-light;
		opacity: 0;
		transition: all 0.25s linear;
		transform: scale(1);
	}

	.mhwi-search-model-show {
		transform: scale(1.2);
		opacity: 1;
	}

	.mhwi-search-bar {
		width: 95%;
		height: 6vh;
		border-radius: 50rpx;
		padding: 0vh 3vh;
		background: #EDEDED;

		.mhwi-search-icon::before {
			content: '\e95c';
			color: $mhwi-primary-light;
			font-size: 3vh;
			margin-right: 1vw;
		}

		.mhwi-search-input {
			font-size: 3vh;
		}

		.mhwi-search-loading-icon::before {
			display: inline-block;
			animation: loading 1.75s linear infinite;
			transform-origin: center center;
			vertical-align: middle;
			content: '\e6de';
			color: #909090;
			font-size: 2.5vh;
		}
	}

	@keyframes loading {
		0% {
			transform: rotate(0deg);
		}

		100% {
			transform: rotate(360deg);
		}
	}

	.mhwi-search-data {
		margin-top: 2vh;

		.mhwi-search-title {
			height: 5vh;
			margin-bottom: 1vh;
		}

		.mhwi-search-item {
			border-top: 5rpx solid #EDEDED;
			border-left: 5rpx solid #EDEDED;
			border-bottom: 5rpx solid #707070;
			border-right: 5rpx solid #707070;
			padding: 1vh;
			margin-bottom: 1.5vh;

			.mhwi-search-item-title {
				color: $mhwi-accent;
				margin-bottom: 1vh;
				font-size: 2vh;
			}

			.mhwi-search-item-info {
				color: #EDEDED;
				font-size: 1vh;
			}
		}
	}

	.mhwi-search-item:hover {
		border: 5rpx solid $mhwi-border-hover;
	}

	.mhwi-search-bottom {
		width: 100%;
		margin-top: 8vh;

		.mhwi-search-slot {}

		.mhwi-search-btn {
			border-radius: 10rpx;
			background: #EDEDED;
			color: $mhwi-primary-light;
			padding: 0.3vw 0.7vw;
			border: 2rpx solid $mhwi-primary-light;
		}
	}

	.mhwi-search-btn:hover {
		background: $mhwi-info-light;
		border: 2rpx solid $mhwi-border-hover;
	}
</style>
