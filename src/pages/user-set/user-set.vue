<template>
	<view>
		<uni-list-item title="账号与安全" @click="open('user-password')"></uni-list-item>
		<uni-list-item title="绑定邮箱" @click="open('user-email')"></uni-list-item>
		<uni-list-item title="资料编辑" @click="open('user-userInfo')"></uni-list-item>
		<uni-list-item title="清除缓存" @click="clear">
			<view slot="right" class="text-muted">{{ currentSize | format}}</view>
		</uni-list-item>
		<uni-list-item title="意见反馈" @click="open('user-feedback')"></uni-list-item>
		<uni-list-item title="关于社区" @click="open('about')"></uni-list-item>
		
		<view class="px-3 py-2">
			<button 
				class="text-white bg-main" 
				style="border-radius: 50rpx; border: 0;" 
			>
				退出登录
			</button>
		</view>
	
	</view>
</template>

<script>
	import uniListItem from '@/components/uni-ui/uni-list-item/uni-list-item.vue';
	export default {
		components: {
			uniListItem
		},
		data() {
			return {
				currentSize: 0 
			}
		},
		onLoad() {
			this.getStorageInfo();
		},
		filters: {
			format(value) {
				// 1024kb = 1M
				return value > 1024 ? (value/1024).toFixed(2) + 'MB' : value.toFixed(2) + 'kb';
			}
		},
		methods: {
			getStorageInfo() {
				let res = uni.getStorageInfoSync();
				this.currentSize = res.currentSize;
			},
			open(path) {
				uni.navigateTo({
					url: `../${path}/${path}`,
				});
			},
			// 清除缓存
			clear() {
				uni.showModal({
					title: '提示',
					content: '是否要清除所有缓存？',
					showCancel: true,
					cancelText: '不清除',
					confirmText: '清除',
					success: res => {
						if (res.confirm) {
							uni.clearStorageSync();
							this.getStorageInfo();
							uni.showToast({
								title: '清除成功',
								icon: 'none'
							});
						}
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>

</style>
