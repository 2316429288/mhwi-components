<template>
	<view class="mhwi-sidebar flex flex-column align-center" :style="{height: $windowHeight + 'px'}"
		:class="show? '':'active'">
		<view v-if="show" class="mhwi-sidebar-head flex align-center justify-between">
			<image src="@/static/image/logo_iceborne.png" mode="widthFix" style="width: 200rpx;" />
			<view class="iconfont arrow-back flex justify-end" @click="show = false" />
		</view>
		<view v-else class="mhwi-sidebar-head flex align-center justify-end">
			<view class="iconfont nav-list flex justify-end" @click="show = true" />
		</view>
		<view v-if="show" class="mhwi-sidebar-nav">
			<scroll-view scroll-y scroll-with-animation :scroll-top="scrollTop" :style="{height: scrollHight}" @scroll="handleViewScroll">
				<view v-for="(item,index) in navList" :key="index"
					class="mhwi-sidebar-item flex justify-between align-center"
					@click="handleNavChange(index)">
					<text class="mhwi-sidebar-nav-title line-1"
						:class="item.active ? 'active': 'disabled'">{{item.title}}</text>
					<mhwi-transition :show="item.active">
						<template v-slot="content">
							<image :src="item.activeIcon" mode="widthFix" style="max-width: 1.5vw;" />
						</template>
					</mhwi-transition>
				</view>
			</scroll-view>
		</view>
		<view v-if="!scrolltoLower && show" class="mhwi-sidebar-arrow" @click="viewScroll">
			<image src="@/static/image/mhwi_arrow_bottom_active.png" mode="widthFix" />
		</view>
	</view>
</template>

<script>
	/**
	  * sidebar 侧边栏
	  * @description 提供一个类似亚克力板的侧边栏
	  * 
	  * @property {Array} navList 数据列表 Array<Object>形式
	  * @property {String} title 数据列表的标题
	  * @property {Boolean} active 数据列表选择项
	  * @property {String} activeIcon 数据列表选中出现的icon
	  * 
	  * @event {Function()} handleNavChange 选择数据时触发，回调参数(index)
	  * @example <mhwi-sidebar :navList="navList" @navClick="handleNavClick" /> 
	  * */
	export default {
		data() {
			return {
				show: true,
				scrollHight: 0,
				scrollTop: 0,
				scrolltoLower: false
			}
		},
		props: {
			navList: {
				type: Array,
				default: []
			}
		},
		methods: {
			handleNavChange(index) {
				this.$emit('navClick', index);
			},
			// 监听屏幕滚动
			handleViewScroll(e) {
				this.scrollTop = e.detail.scrollTop
				if(this.scrollTop > 200) {
					this.scrolltoLower = true;
				} else {
					this.scrolltoLower = false;
				}
			},
			// 去往底部按钮点击，给一个很大的值
			viewScroll() {
				this.scrollTop = 1500
			}
		},
		mounted() {
			// 获取头部高度，计算scroll-view高度
			this.$mhwi.getRect('.mhwi-sidebar-head').then(res => {
				this.scrollHight = this.$windowHeight - res.height / 2 + 'px';
			});
		}
	}
</script>

<style lang="scss">
	.mhwi-sidebar {
		width: 15%;
		background: #ededed;
		opacity: 80%;
		padding: 0rpx 20rpx;
		transition: transform 1s;
		transform: translateX(0rpx);
	}

	.mhwi-sidebar.active {
		transform: translateX(-300rpx);
	}

	.mhwi-sidebar-head {
		width: 100%;
		margin: 1vh 0;

		.arrow-back,
		.nav-list {
			color: $mhwi-primary;
			font-size: 2vw;
		}

		.arrow-back::before {
			content: '\e918';
		}

		.nav-list::before {
			content: '\e8b8';
			margin-right: 50rpx;
		}
	}

	.mhwi-sidebar-item {
		width: 8vw;
		height: 5vh;
		margin-bottom: 1vh;
	}

	.mhwi-sidebar-nav-title {
		font-size: 1vw;
		font-weight: 600;
	}

	.mhwi-sidebar-item :hover {
		color: $mhwi-primary;
	}

	.mhwi-sidebar-nav-title.active {
		color: $mhwi-primary;
	}

	.mhwi-sidebar-nav-title.disabled {
		color: $mhwi-text;
	}
	
	.mhwi-sidebar-arrow {
		position: fixed;
		bottom: 5vh;
		left: 1vw;
		animation: shake linear 0.5s infinite alternate;
		image {
			width: 1.5vw;
		}
	}
	@keyframes shake {
		0% {
			transform: translateY(-3vh);
		}
		100% {
			transform: translateY(0vh);
		}
	}
</style>
