<template>
	<view class="newsbox">
		   <view class="pic">
		   	<image :src="item.picurl" mode="aspectFit"></image>
		   </view>
		   <view class="text">
		   	<view class="title">
		   		{{item.title}}
		   	</view>
			<view class="info" v-if="!item.looktime">
				<text>{{item.author}}</text>
				<text>/{{item.hits}}阅读</text>
			</view>
			<view class="info" v-else>
				<text>浏览时间:{{item.looktime}}</text>
			</view>
		   </view>
	</view>
</template>

<script>
	export default {
		name:"newsbox",
		props:{
			item:{
				type:Object,
				default(){
					return{
						title:"组件内默认的标题",
						author:"张三",
						hits:668,
						picurl:"../../static/image/tabbar/me-h.png"
					}
				}
			}
		},
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">
.newsbox{
	display: flex;
	background-color: #efefef;
	border-radius: 20rpx;
	margin-top: -20rpx;
	.pic{
		width: 230rpx;
		height: 160rpx;
		image{
			width: 100%;
			height: 100%;
		}
	}
	.text{
		
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		// 占满全屏的宽
		flex: 1; 
		padding-left: 28rpx;
		.title{
			font-size: 36rpx;
			color: #333;
			overflow: hidden;
			text-overflow: ellipsis;
			display: -webkit-box;
			-webkit-line-clamp: 2;
			-webkit-box-orient: vertical;
		}
		.info{
			font-size: 26rpx;
			color: #999;
			
		}
	}
}
</style>