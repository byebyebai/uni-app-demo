<template>
	<view>
		<view class="textBox" :style="[{'background-color':bgcolor}]" @click="MenuGroupAction">
			<view class='text' :style="[{'font-size': Size+'px',animation:'animateText '+animateTime+'s infinite linear',color:color}]"> {{bulletMessage}} </view>
		</view>
		<view class="inputBox" v-if="MenuGroup">
		  <input class="inp" placeholder="请输入弹幕~限制30字之内哦" v-model="newbulletMessage" cursor-spacing='10'></input>
		  <view @click="sendBtn" class="iconfont icon-1huojian iconBtn1">发送</view>
		  <view @click="showModal" class="iconfont icon-qita3 iconBtn1">属性</view>
		</view>
		<view class="commodity_screen" @click="hideModal" v-if="showModalStatus"></view>
		<!-- 屏幕内容 -->
		<view animation="animationData" class="commodity_attr_box"  v-if="showModalStatus">
			<view class="swiper-tab swiperAttr">
				<view class="iconfont icon-jurassic_font-sizeadd swiper-tab-item" :class="{active:1==currentTab}" @click="clickTab(1)">字号</view>
				<view class="iconfont icon-yanse1 swiper-tab-item" :class="{active:2==currentTab}" @click="clickTab(2)">颜色</view>
				<view class="iconfont icon-Group- swiper-tab-item" :class="{active:3==currentTab}" @click="clickTab(3)">速度</view>
				<view class="iconfont icon-beijingse swiper-tab-item" :class="{active:4==currentTab}" @click="clickTab(4)">背景</view>
			</view>
			<swiper duration="300" :current="currentId" class="swiper" v-if="showModalStatus" @change="swiperTab">
			    
				<!-- 字体大小 -->
				<swiper-item>
				  <view class="swiperItem1">
					<slider :value='Size' @change="changeFontSize" selected-color='#006AFE'></slider>
				  </view>
				</swiper-item>
				<!-- 选择颜色 -->
				<swiper-item>
				  <view class="swiperItem2">
					
					<view class="colorBox">
						<view class="colorItems" :class="{checked:item.color == color}"  v-for="(item,index) in colorArr" :key="index" :style="{'background-color':item.color}" @click="changeColor(item.color)"></view>
					</view>
				  </view>
				</swiper-item>
				<!-- 字体速度 -->
				<swiper-item>
					<view class="swiperItem1">
						<slider @click='changeTextSpeend' selected-color='#006AFE' :value='animateTime'></slider>
					</view>
				</swiper-item>
				<!-- 背景颜色 -->
				<swiper-item>
				  <view class="swiperItem2">
					<view class="colorBox" bindtap="setBackGroundColor">
						
					    <view class="colorItems" :class="{checked:item.color == bgcolor}" v-for="(item,index) in colorArr" :key="index" :style="{'background-color':item.color}" @click="changeBgColor(item.color)"></view>
					</view>
				  </view>
				</swiper-item>
			</swiper>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				bulletMessage:"请输入弹幕~限制30字之内哦",
				newbulletMessage:"",
				Size:65, //字体大小控制
				animateTime:10, //滚动效率控制
				sliderValOfAnimateTime:0, //进度条数据
				color:'white', //默认颜色
				bgcolor:'black', //默认背景色
				showModalStatus:false, //模态框开关 默认关闭
				animationData:false, 
				currentTab:1,
				currentId:0,
				MenuGroup:true,
				colorArr:[
				      { color: 'pink' },
				      { color: "red" },
				      { color: "blue" },
				      { color: "yellow" },
				      { color: "white" },
				      { color: "aqua" },
				      { color: "green" },
				      { color: "skyblue" },
				      { color: "hotpink" },
				      { color: "black" }
				    ]
			};
		},
		onReady() {

		},
		methods:{
			MenuGroupAction(){
				this.MenuGroup = !this.MenuGroup
			},
			//改变背景色
			changeBgColor(color){
				this.bgcolor = color
			},
			//改变播放速度
			changeTextSpeend(e){
				//50 默认 10s
				//0 是 15s
				//100 是 5s
				let sliderVal = e.detail.value;
				
				this.animateTime = sliderVal * -0.1 + 15
				sliderValOfAnimateTime = sliderVal
			},
			//改变字体颜色
			changeColor(color){
				this.color = color
			},
			//改变字体大小
			changeFontSize(e){
				this.Size = e.detail.value
			},
			//发送弹幕
			sendBtn(){
				if(this.newbulletMessage == ''){
					return
				}
				if(this.newbulletMessage.length>30){
					this.newbulletMessage = this.newbulletMessage.slice(0,25);
				}
				this.bulletMessage = this.newbulletMessage
				this.newbulletMessage = ''
			},
			//显示设置项
			showModal(){
				this.showModalStatus = true
				this.animationData = true
			},
			//设置项菜单切换
			clickTab(current){
				if(current == this.currentTab){
					return false;
				}
				this.currentTab = current
				this.currentId = current-1
			},
			//设置项轮播绑定
			swiperTab(e){
				this.currentTab = e.detail.current+1
			},
			//隐藏模态框
			hideModal(){
				setTimeout(()=>{
					this.showModalStatus = false
				},200)
				
			}
		}
		
	}
</script>

<style lang="scss">
	.textBox{
	  height: 100vh;
	  display: flex;
	  justify-content: center;
	  background-color: black;
	  position: relative;
	}
	.checked{
		border: 2rpx solid #FFFFFF;
	}
	.text{
	  transform:rotate(90deg);
	  height: 1rpx;
	  display: flex;
	  align-items: center;
	  white-space: nowrap;
	  position: fixed;
	  top: 280%;
	  color: white;
	}
	@keyframes animateText{
	  0%{
	    margin-top: 0%;
	  }
	  100%{
	    margin-top: -700%;
	  }
	  
	}
	.inputBox{
	  position: fixed;
	  bottom: 1%;
	  display: flex;
	  /* background-color: saddlebrown; */
	}
	.inp{
	  border: 1px #333333 solid;
	  border-radius: 50rpx;
	  margin-left: 30rpx;
	  padding-left: 20rpx;
	  color: white;
	  font-size: 30rpx;
	  width: 390rpx;
	  height: 63rpx;
	}
	.iconBtn1{
	  /* border: 1px white solid; */
	  width: 130rpx;
	  height: 70rpx;
	  border-radius: 60rpx;
	  display: flex;
	  justify-content: center;
	  align-items: center;
	  font-size: 30rpx;
	  font-weight: bold;
	  background-color: #333333;
	  color: white;
	  margin-left: 10rpx;
	  
	}
	
	.swiperItem1{
	  padding-top: 15%;
	  padding-left: 5%;
	}
	.swiperItem2{
	  padding-top: 15%;
	}
	
	
	/* 色块 */
	.colorBox{
	  display: flex;
	  justify-content: space-evenly;
	}
	.colorItems{
	  width: 50rpx;
	  height: 50rpx;
	}
	/* 弹起框的样式 */
	/*使屏幕变暗 */
	.commodity_screen {
	 width: 100%;
	 height: 100%;
	 position: fixed;
	 top: 0;
	 left: 0;
	 background: #000;
	 opacity: 0.8;
	 overflow: hidden;
	 z-index: 1000;
	 color: #fff;
	}
	/*对话框 */
	.commodity_attr_box {
	 height: 430rpx;
	 width: 100%;
	 overflow: hidden;
	 position: fixed;
	 bottom: 0;
	 left: 0;
	 z-index: 2000;
	 background: #282828;
	 border-radius: 10rpx 10rpx 0 0;
	}
	
	/* swiper start */
	
	.swiper-tab{
	    width: 100%;
	    border-bottom: 2rpx solid #373737;
	    text-align: center;
	    height: 88rpx;
	    line-height: 88rpx;
	    font-weight: bold;
	    background-color: #282828;
	}
	
	.swiper-tab-item{
	    display: inline-block;
	    width: 25%;
	    color:#939393;  
	    background-color: #282828;
	}
	.active{
	    color:white;
	    border-bottom: 4rpx solid#373737;
	}
	swiper{
	  color: white;
	  background-color: #282828;
	}
	
	/* swiper end */
</style>
