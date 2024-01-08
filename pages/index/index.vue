<template>
	<view class="home">
		<view class="scrollNav">
			<scroll-view scroll-x class="navscroll">
			<view class="item" :class="index==navIndex ? 'active' : ''" v-for="(item,index) in navArr" @click="item.id!=id && clickNav(index,item.id)" :key="item.id">
			{{item.classname}}</view>
			</scroll-view>
		</view>
		
		<view class="content">
			<view class="row" v-for="item in newsArr">
				<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
				
			</view> 
		</view>
		<view class="nodata" v-if="!newsArr.length">
			<image src="../../static/image/tabbar/message.png" mode="widthFix"></image>
		</view>
		<view class="loading" v-if="newsArr.length">
			<view v-if="loading==1">
				数据加载中~~~
			</view>
			<view v-if="loading==2">
				已经没有更多数据了
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex:0,
				navArr:[],
				newsArr:[],
				currentPage:1,
				id:50,
				loading:0 //loading  0为默认  1 为加载中  2 为没数据了
			}
		},
		onLoad() {
		this.getNavData();
		this.getNewsData(this.id);
		},
		onReachBottom() {
			
			console.log("到底部了")
			if(this.loading==2){
				return;
			}
			this.loading=1,
			this.currentPage++;
			this.getNewsData(this.id); 
			
		},
		methods: {
		// 点击导航切换
		clickNav(index,id){
		this.navIndex=index,
		this.currentPage=1;
		this.newsArr=[]
		this.id=id
		this.getNewsData(this.id),
		
		
		this.loading=0;
		
		
		},
		goDetail(item){
			uni.navigateTo({
				url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
			})
		},
		// 获取导航列表数据
		getNavData(){
			uni.request({
				url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
				success:res=>{
					console.log(res)
					this.navArr=res.data
				}
			})
		},
		// 获取新闻列表数据
		getNewsData(id){
			uni.request({
				url:"https://ku.qingnian8.com/dataApi/news/newslist.php",
				data:{
					cid:id,
					page:this.currentPage
				},
				success:res=>{
					console.log(res)
					if(res.data.length==0){
						this.loading=2
					}
					this.newsArr=[...this.newsArr,...res.data]
				
					
				}
			})
		}
		}
	}
</script>

<style lang="scss" scoped>
// 滚动导航栏
.navscroll{
	height: 70rpx;
	background-color: lightgray;
	white-space: nowrap;
	position: fixed;
	top: var(--window-top);
	z-index: 10;
	// 把底部滚动条弄没
	// 滚动条样式
	/deep/ ::-webkit-scrollbar{
		width: 4px !important;
		height: 1px !important;
		overflow: auto !important;
		background: transparent !important;
		display: block;
	}
	.item{
		font-size: 40rpx;
		display: inline-block;
		line-height: 70rpx;
		padding: 0 30rpx;
		color: #333;
		&.active{
			color: darkred;
		}
	}
}

// 新闻内容样式
.content{
	padding: 30rpx;
	padding-top: 100rpx;
	.row{
		border-bottom: 1px dashed #efefef;
		padding: 15rpx 0;
	}
}
.nodata{
	display: flex;
	justify-content: center;
	image{
		width: 200rpx;
		height: 200rpx;
	}
	
}
.loading{
		margin-top: -10rpx;
		text-align: center;
		font-size: 10rpx;
		color: deeppink;
		line-height: 3em;
		margin-bottom: 30rpx;
}
</style>
