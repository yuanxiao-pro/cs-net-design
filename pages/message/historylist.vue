<template>
	<view class="">
		<view class="">
			<u-card :title="item.ip" :sub-title="subTitle" :thumb="thumb" 
			margin="60rpx"
			v-for="(item,index) in Pings" :key="index" 
			v-if="item.connected == 1 ? subTitle = 'PONG':subTitle = 'ERROR'"
			@click="showDetail(index)"
			>
				<view class="" slot="body">
					<view class="u-body-item u-flex u-border-bottom u-col-between u-p-t-0">
						<view class="u-body-item-title u-line-2">
							<!-- {{item.pong}} -->
							<scroll-view scroll-y="true" style="height: 240rpx;">
								<view>
									{{item.pong}}
								</view>
							</scroll-view>
						</view>
					</view>
				</view>
			</u-card>
		</view>
		
	</view>
	
</template>

<script>
	var app = getApp();
	export default {
		data() {
			return {
				title: '测试结果',
				subTitle: 'ERROR',
				thumb: '/static/img/time.png',
				showPopup:false,
				/* 数据模型 */
				Pings:[],
			};
		},
		methods: {
			onPullDownRefresh() {
				this.getPings();
				setTimeout(()=>{
					uni.stopPullDownRefresh();
				},1000)
			},
			onLoad(pages){
				this.getPings();
			},
			
			getPings(){
				uni.request({
					method: 'POST',
					header: {
						'content-type': 'application/x-www-form-urlencoded', 
					},
					//IP地址应该为本机的ipv4地址，不能是localhost或者127.0.0.1
					url: app.globalData.url+'/getAllPings', 
					dataType:'json',
					data: {
					},
					success: (res) => {
						
						console.log(res.data.status);
						if(res.data.status == 100){
							this.Pings = res.data.value; // = this.Pings;
							// console.log(this.Pings)
						} else if(res.data.status == 200){
							console.log(res.data.value);
							uni.showToast({
								icon:'none',
								title:'暂无数据！'
							});
						}
					},
					fail(){
						uni.showToast({
							icon:'loading',
							title:'网络异常!'
						});
					}
				});
			},
			
			
		}
	};
</script>

<style scoped lang="scss">
	.u-card-wrap { 
		background-color: $u-bg-color;
		padding: 1px;
	}
	
	.u-body-item {
		font-size: 32rpx;
		color: #333;
		padding: 20rpx 10rpx;
	}
		
	.u-body-item image {
		width: 120rpx;
		flex: 0 0 120rpx;
		height: 120rpx;
		border-radius: 8rpx;
		margin-left: 12rpx;
	}
</style>