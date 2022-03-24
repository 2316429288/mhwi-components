<template>
	<view>
			<uni-transition :mode-class="mode" :show="show">
				<slot></slot>
			</uni-transition>
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
