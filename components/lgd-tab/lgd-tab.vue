<template>
	<view>
		<scroll-view class="scrollView" scroll-x show-scrollbar="false" :scroll-left="scrollLeft" scroll-with-animation>
			<view class="tabBox" :style="{ 'justify-content': isOutWindow ? '' : 'space-around' }">
				<view class="items" v-for="(item, index) in tabValue" :key="index" @click="clickTab(index)">
					<text class="tabText" :class="index == tIndex ? 'active' : ''" :style="{ 'font-size': fontSize + 'px', color: index == tIndex ? textColor : ''}">
						{{item}}
					</text>
				</view>
			</view>
			<view class="underscore" :style="{ width: inderWidth + 'px', 'margin-left': indexLeft + boxLeft + 'px', 'background-color': textColor, height: '3px' }" />
		</scroll-view>
	</view>
</template>

<script>
	export default {
		props: {
			tabValue: { // tab数据
				type: Array,
				default: [],
				required: true
			},
			textColor: { // 颜色
				type: String,
				default: '#34b2fa'
			},
			fontSize: { // 字体大小
				type: Number,
				default: 16
			}
		},
		data() {
			return {
				windowsWidth: 0,
				boxLeft: 0,
				isOutWindow: false,
				inderWidth: 0,
				indexLeft: 0,
				scrollLeft: 0,
				tIndex: 0,
			};
		},
		methods: {
			clickTab(index) {
				// 更改选中下标
				this.tIndex = index
				this.$emit("getIndex", index)
				// 获取盒子移动距离
				if (this.isOutWindow && index >= 0) {
					uni.createSelectorQuery().in(this).select(".tabBox").boundingClientRect().exec((data) => {
						if (index == 0) {
							this.boxLeft = 0
						} else {
							// 移动距离
							this.boxLeft = -data[0].left
						}
					})
				}
				uni.createSelectorQuery().in(this).selectAll(".items").boundingClientRect().exec((data) => {
					let width = data[0][index].width
					let left = data[0][index].left
					let newLeft = 0

					// tab超出屏幕
					if (this.isOutWindow) {
						if (index == 0) {
							left = 0
						}

						// 防止移动后距离缩短问题
						for (let i = 0; i < index; i++) {
							newLeft += data[0][i].width
						}

						// 是否需要居中
						if (this.windowsWidth / 2 < newLeft + width) {
							this.scrollLeft = width / 2 - (this.windowsWidth / 2 - newLeft)
						} else {
							this.scrollLeft = 0
						}
					}
					// 点击tab宽度
					this.inderWidth = width / 2
					// 移动距离
					this.indexLeft = (width - width / 2) / 2 + left
				})
			}
		},
		created() {
			let that = this;
			// 获取屏幕宽度
			uni.getSystemInfo({
				success(res) {
					that.windowsWidth = res.windowWidth
				}
			})
		},
		mounted() {
			// 判断tab是否超出屏幕
			uni.createSelectorQuery().in(this).selectAll(".items").boundingClientRect().exec((data) => {
				if (data[0][this.tabValue.length - 1].right > this.windowsWidth) {
					this.isOutWindow = true
				}
			})
			this.clickTab(0)
		}
	}
</script>

<style>
	.scrollView {
		width: 100%;
		white-space: nowrap;
	}

	.tabBox {
		display: flex;
		align-items: center;
	}

	.items {
		padding: 6px 20rpx;
	}

	.tabText {
		color: #666666;
	}

	.active {
		font-weight: bold;
	}

	.underscore {
		transition: .2s all;
	}
</style>
