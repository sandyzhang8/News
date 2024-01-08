<template>
	<view>
		<view class="detail">
			<view class="title">
				{{detail.title}}
			</view>
			<view class="info">
				<view class="author">
					编辑：{{detail.author}}
				</view>
				<view class="time">
					发布时间{{detail.posttime}}
				</view>
			</view>
			<!-- 富文本解析html的 -->
			<view class="content">
				
			
			<rich-text :nodes="detail.content"></rich-text>
			</view>
			<view class="description">
				声明：与我无关与我无关与我无关与我无关与我无关
			</view>
		</view>
	</view>
</template>

<script>
	import {timestampToTime} from "@/utils/tool.js" //引入js 转换10位时间戳
	
	import {timestampToTime2} from "@/utils/tool.js" //引入js 转换13位时间戳
	export default {
		data() {
			return {
				options:null,
				detail:{}
			};
		},
		onLoad(e) {
			console.log(e),
			this.options=e;
			this.getDetail();
		},
		methods:{
			getDetail(){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/detail.php",
					data:this.options,
					success:res=>{
						
						res.data.posttime=timestampToTime(res.data.posttime)
						res.data.content=res.data.content.replace(/<img/gi,'<img style="max-width:100%"') //正则，类似control+f查找替换，但原本img有style的话就不起作用
						
						this.detail=res.data
						this.saveHistory()
					}
				})
			},
			saveHistory(){
				let historyArr=uni.getStorageSync("historyArr") || []
				
				
				let item={
					id:this.detail.id,
					classid:this.detail.classid,
					title:this.detail.title,
					picurl:this.detail.picurl,
					looktime:timestampToTime2(Date.now())
				}
				
				let index=historyArr.findIndex(i=>{
					return i.id==this.detail.id
				})
				
				if(index>=0){
					historyArr.splice(index,1)
				}
				historyArr.unshift(item)
				
				uni.setStorageSync("historyArr",historyArr)
			}
		}
	}
</script>

<style lang="scss">
.detail{
	padding: 30rpx;
	.title{
		font-size: 46rpx;
		color: #333;
	}
	.info{
		background-color: #f6f6f6;
		padding: 28rpx 10rpx;
		font-size: 25rpx;
		color: #666;
		display: flex;
		justify-content: space-between;
		margin: 40rpx 0;
	}
	.content{
		
		
	}
	.description{
		background-color: #ff0;
		font-size: 25rpx;
		margin-top: 25rpx;
		color: palevioletred;
		line-height: 1.8em;
	}
}
</style>
