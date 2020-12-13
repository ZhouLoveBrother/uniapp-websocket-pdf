<template>
	<view class="container">
		<!-- 左侧  需求:通过websocket实时获取数据，赋值到输入框 ，如果websocket链接失败则弹窗警告-->
		  <view class="content-left">
			     <!-- 扫描条码 -->
				 <view class="textFontSize scan-container">
						<view class="mr-2 color-red font-w-b" style="font-size: 35rpx;">扫描条码:</view>
						<view class="scanBox flex-1 mr-1">
								<input class="flex-1 pl-2" type="text" confirm-type="search" placeholder="请扫描载具条码" v-model="scanCode"/>
						</view>
						<view class="scan-btn">
							<text class="iconfont icon-dagou"></text>
							<text class="ml-2">扫描提交</text>
						</view>
				 </view>
				 
		  </view>
		 
		
		<!-- 右侧 -->
		 <view class="content-right">
			   <!-- pdf文件预览 -->
			   <view class="pdf-box" >
					<image src="../../static/images/pdf.jpg"  class="pdf-img" @click="showPdf"></image>
				</view>
			 
		 </view>
		
		   <!-- websocket连接失败弹窗 -->
		   <u-mask :show="socketShow" >
		   		<view class="warp">
		   			<u-tag :text="webtext" mode="dark" class="warp-tag" type="error"/>
		   		</view>
		   	</u-mask>
		
		<u-toast ref="uToast" />
		
	</view>
</template>
<script>

import { websocetObj } from '@/utils/websocet/websocet.js';

export default {
data() {
	return {
		  socketShow:false,
			webtext:'',
			
	  }
},
		methods: {
			
			//pdf,传入pdf的url 可实现在线预览
			showPdf(){
				   let links = encodeURIComponent('http://rtdsoft.rtdsoft.cn:6100/files/sop/file_befca6a4.pdf')
				        uni.navigateTo({
						  url:'../pdfWebview/pdfWebview?links='+ links
				    })
			},
			
			
			//websocet函数回调：返回监听的数据
			getWebsocetData(val){
				console.log(val,'函数回调');
				this.scanCode = val;
			},
			//websocet函数抛错： 返回错误信息 用于用户提示
			getWebsocetError(err){
				this.socketShow = err.isShow;
			    this.webtext = err.messge;
				console.log('websocet函数抛错',this.socketShow);
			},
			//websocet函数成功进入： 监听连接状态，在失败的时候弹窗提示，具体需求看自身情况
			onErrorSucceed(val){
				this.socketShow = val.isShow;
				console.log('websocet函数成功进入',this.socketShow);
			}
			   
		},
		mounted() {
		
		},
		onLoad() {
	    // 在onload的时候调用，创建webscoet连接对象，参数分别为：url、获取后端返回数据、监听websocket的链接失败返回的报错、监听链接状态，返回布尔值
			websocetObj.sokcet('ws://192.168.1.106:8080',this.getWebsocetData,this.getWebsocetError,this.onErrorSucceed)
		
    },
    //离开页面销毁websocket
		beforeDestroy() {
			websocetObj.stop();
	   },
		
		
	}
</script>

<style scoped lang="less">
.container{
	display: flex;
	box-sizing: border-box;
	padding: 18rpx 15rpx 18rpx 15rpx;
}	
.content-left{
	width: 60vw;
	overflow: hidden;
}
.content-right{
	width: 35vw;
	overflow: hidden;
	margin-left: 40rpx;
	
}

	/**扫描条码 */
	.scan-container{
		display: flex;
		align-items: center;
		margin-top: 20rpx;
		padding-bottom: 15rpx;
		border-bottom: 5rpx solid #bbb;
	}
	.textFontSize{
		font-size: 25rpx !important;
	}
	.scanBox{
		display: flex;
		align-items: center;
		justify-content: space-between;
		border: 2rpx solid #bbb;
		margin-top: 10rpx;
		padding: 5rpx 0;
	}
	.scan-btn{
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: #4ccfff;
		border-radius: 16rpx;
		width: 200rpx;
		height: 60rpx;
	}

	/** pdf*/
	.pdf-box{
		height: 45vh;
		width: 100%;
	}
	.pdf-img{
		width: 100%;
		height: 100%;
	}
	

	/**web-view*/
	.warp{
	  display: flex;	
	  justify-content: center;
	  align-items: center;
	  position: fixed;
	  left: 0;
	  right: 0;
	  top: 0;
	  bottom: 0;
	}
    .warp-tag{
		font-size: 80rpx;
	}
</style>
