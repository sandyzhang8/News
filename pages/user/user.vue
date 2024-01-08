<template>
	<view class="user">
		<view class="top">
			<image src="../../static/image/tabbar/me-h.png" mode=""></image>
		    <view class="text">
		    	浏览历史
		    </view>
		</view>
		
		<view class="content" v-if="listArr.length">
			<view class="row" v-for="item in listArr">
				<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
				
			</view>
		</view>
		
		<view class="nohistory" v-else>
			<image src="../../static/image/tabbar/create-h.png" mode="widthFix"></image>
			<text>暂无浏览记录</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				 listArr:[]
			};
		},
		onShow() {
			this.getData()
		},
		methods:{
			goDetail(item){
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			getData(){
				let hisArr=uni.getStorageSync("historyArr") || []
				this.listArr=hisArr
			}
		}
	}
</script>

<style lang="scss">
.user{
	.top{
		padding-top: 50rpx 0;
		background-color: #f8f8f8;
		color: #666;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 100rpx;
			height: 100rpx;
		}
		.text{
			font-size: 38rpx;
			padding-top: 10rpx;
		}
		
	}
	.content{
		padding: 30rpx;
		.row{
			border-bottom: 1px dashed #efefef;
			padding: 15rpx 0;
		}
	}
		
	.nohistory{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 200rpx;
		}
		text{
			font-size: 55rpx;
			color: #666;
		}
	}
}
</style>
