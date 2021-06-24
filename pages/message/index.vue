<!-- 项目说明 -->
<template>
	<view class="wrap">
		<view class="form-content">
			<view class="project-info-content">
				<!-- 项目说明页面 -->
				<u-form :model="Ping" ref="uForm" :border-bottom=false >
					
					<u-form-item label="目标网址:" label-width="150" prop="ip" :border-bottom=true>
						<u-input class="project-input"  placeholder="请输入网址或IP地址" v-model="Ping.ip" />
					</u-form-item>
					
					<!-- <u-form-item label="超时时间:" label-width="150" prop="ttl" :border-bottom=true>
						<u-input class="project-input" placeholder="" v-model="Ping.ttl" />
					</u-form-item> -->
					
					
				</u-form>
			</view>
		</view>
		<u-button class="info-btn" type="primary" @click="submit">Ping</u-button>
	</view>
</template>

<script>
	var app = getApp();
	export default{
		data() {
			return {
				/* 数据模型 */
				Ping:{
					ip:'',
					// ttl: 0,
				},
				
				/* 校验规则 */
				rules: {
					
				}
			}
		},
		
		methods: {
			//提交ping请求
			submit(){
				// console.log(app.globalData.url);
				uni.request({
					method: 'POST',
					header: {
						'content-type': 'application/x-www-form-urlencoded', 
					},
					//IP地址应该为本机的ipv4地址，不能是localhost或者127.0.0.1
					url: app.globalData.url+'/ping', 
					dataType:'json',
					data: {
						// ip: this.ip,
						ip: this.Ping.ip,
						// ttl: thi.ttl
					},
					success: (res) => {
						
						console.log(res.data.status);
						if(res.data.status == 100){
							console.log(res.data.value);
							uni.showToast({
								icon:'success',
								title:'PONG!'
							});
							uni.preloadPage({url: "/pages/message/historylist"});
							uni.switchTab({
								url:'/pages/message/historylist',
								
							});
						} else if(res.data.status == 200){
							console.log(res.data.value);
							uni.showToast({
								icon:'none',
								title:'ERROR！'
							});
							uni.switchTab({
								url:'/pages/message/historylist',
							});
						}
					},
					fail(){
						uni.showToast({
							icon:'loading',
							title:'网络异常!'
						});
						console.log("网络异常!");
					}
				});
			}
		},
	}
</script>

<style lang="scss" scoped>
	.warp{
		padding: 40rpx;
	}
	.form-content{
		padding: 40rpx;	width: 80%;
		margin: auto;
		margin-top: 320rpx;
		// background-color: #000000;
		border-radius: 20rpx;
		box-shadow:0px 0px 10px #888;
	}
	.project-input{
		/* 去除input框的上左右边框 */
		border-top: none;
		border-left: none;
		border-right: none;
		border-radius: 0rpx;
		// width: 90%;
		// float: right;
	}
	.info-btn{
		margin-top: 50px;
		width: 80%;
		font-size: x-small;
		color: #fff;
	}
</style>

