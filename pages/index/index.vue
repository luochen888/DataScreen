<template>
	<view class="container" >
		<view class="left">
			<view class="left_top">
				<view class="title">
					<image src="../../static/1-1.png" mode=""></image>
					<text>库房数据</text>
					<image src="../../static/1-2.png" mode=""></image>
				</view>
				<view class="left_top_info">
					<text>装备数量</text>
				</view>
				<view class="left_top_num">
					<text v-for="(item1,index) in equipmentTitalNum" :key="index" :class="[equipmentTitalNum[index]==','?'activeText':'']">{{equipmentTitalNum[index]}}</text>
				</view>
			</view>
			<view class="left_center">
				<view class="title">
					<image src="../../static/1-1.png" mode=""></image>
					<text>月度库存物资分类对比</text>
					<image src="../../static/1-2.png" mode=""></image>
				</view>
				<view class="qiun-columns">
						<view class="qiun-bg-white qiun-title-bar qiun-common-mt">
						</view>
						<view class="qiun-charts">
							<canvas canvas-id="canvasRing" id="canvasRing" class="charts"></canvas>
						</view>
				</view>

			</view>
			<view class="left_bottom">
				<view class="title">
					<image src="../../static/1-1.png" mode=""></image>
					<text>每周出入库统计</text>
					<image src="../../static/1-2.png" mode=""></image>
				</view>
				<view class="qiun-columns">
					<view class="qiun-bg-white qiun-title-bar qiun-common-mt" >
						
					</view>
					<view class="qiun-charts1" style="padding-top: 8px; margin-left: 30px;">
						<canvas canvas-id="canvasColumn" id="canvasColumn" class="charts" ></canvas>
					</view>
				</view>
			</view>
		</view>
		<view class="center">
			<image src="../../static/center2.gif" mode="" style="width: 100%; height: 105%;"></image>
		</view>
		<view class="right">
			<view class="right_top">
				 <view class="title2">
					<image src="../../static/1-1.png" mode=""></image>
					<text>库位利用占比</text>
					<image src="../../static/1-2.png" mode=""></image>
				</view>
				<view class="left_top_info">
					<text>库位总量</text>
				</view>
				<view class="left_top_num">
					<text v-for="(item1,index) in locationTitalNum" :key="index" :class="[equipmentTitalNum[index]==','?'activeText':'']">{{locationTitalNum[index]}}</text>
				</view>
				<view class="right_progressbar">
					<view class="right_progressbar_auto">
						<text>公共库位</text>
					 <view class="progress-box" >
						<progress :percent='this.PublicPoistionCount' activeColor="#0FC9EB" show-info active stroke-width="15" backgroundColor='#020A34' />
					</view>
					
					</view>
					<view class="right_progressbar_auto">
						<text>单警柜</text>
					<view class="progress-box">
						<progress :percent='this.PoistionCount' activeColor="#0FC9EB" show-info active stroke-width="15" backgroundColor='#020A34'/>
					</view>
					</view>
					
				</view>
			</view>
			<view class="right_bottom">
				<view class="title2">
					<image src="../../static/1-1.png" mode=""></image>
					<text>出入库记录</text>
					<image src="../../static/1-2.png" mode=""></image>
				</view>
				<view class="right_bottom_list">
					<ul>
						<li>
							<text>时间</text>
							<text>操作人</text>
							<text>操作</text>
							<text>装备</text>
						</li>
					</ul>
					<ul class="right_bottom_list_ul">
						 <view class="uni-padding-wrap" >
						            <view class="page-section swiper" >
						                <view class="page-section-spacing" >
						                    <swiper class="swiper  right_bottom_list_ul_li" style="height: 280px;" 
											 :autoplay="autoplay" :interval="interval" 
											:duration="duration" display-multiple-items="10" circular="true" vertical="true">
						                        <swiper-item v-for="(item,index) in realData" :key='index'  class="right_bottom_list_swiper">
						                           <text>{{item.InSpectionTime.replace('T','  ')}}</text>
						                           <text>{{item.Operation}}</text>
						                           <text>{{item.OperationType}}</text>
						                           <text>{{item.Name}}</text>
						                        </swiper-item>
						                    </swiper>
						                </view>
						            </view>
						        </view>
					</ul>
					
				 </view>
			</view>
		</view>
		<view class="Public_equipment" v-show="Public_equipmentIsshow">
			<view class="title2">
				<image src="../../static/1-1.png" mode=""></image>
				<text>公共装备</text>
				<image src="../../static/1-2.png" mode=""></image>
			</view>
			<view class="Public_equipment_list">
				<ul>
					<li>
						
						<text>装备名称</text>
						<text>货位号</text>
						<text>库存量</text>
					</li>
				</ul>
				<ul class="Public_equipment_list_info">
					<li v-for="(item,index) in PublicData" :key="index">
						
						<text>{{item.Name  }}</text>
						<text>{{item.Location }}</text>
						<text>{{item.Stocknum }}</text>
					</li>
				</ul>
			</view>
		</view>
		<view class="Single_equipment" v-show="Single_equipmentIsshow">
			<view class="title2">
				<image src="../../static/1-1.png" mode=""></image>
				<text>单警装备</text>
				<image src="../../static/1-2.png" mode=""></image>
			</view>
			<view class="Single_equipment_list">
				<ul>
					<li>
						
						<text>装备名称</text>
						<text>货位号</text>
						<text>库存量</text>
					</li>
				</ul>
				<ul class="Public_equipment_list_info">
					<li v-for="(item,index) in SingleData" :key="index">
						
						<text>{{item.Name  }}</text>
						<text>{{item.Location }}</text>
						<text>{{item.Stocknum }}</text>
					</li>
				</ul>
			</view>
		</view>
		<view class="Emergency_equipment" v-show="Emergency_equipmentIsshow">
			<view class="title2">
				<image src="../../static/1-1.png" mode=""></image>
				<text>应急装备</text>
				<image src="../../static/1-2.png" mode=""></image>
			</view>
			<view class="Emergency_equipment_list">
				<ul>
					<li>
						
						<text>装备名称</text>
						<text>货位号</text>
						<text>库存量</text>
					</li>
				</ul>
				<ul class="Public_equipment_list_info">
					<li v-for="(item,index) in EmergencyData" :key="index">
					
						<text>{{item.Name}}</text>
						<text>{{item.Location }}</text>
						<text>{{item.Stocknum }}</text>
					</li>
				</ul>
			</view>
		</view>
		<view class="Public_equipment_click" @tap="Public_equipment_click" data-id="公共装备"></view>
		<view class="Single_equipment_click" @tap="Single_equipment_click" data-id="单警装备"></view>
		<view class="Emergency_equipment_click" @tap="Emergency_equipment_click" data-id="应急装备"></view>
		<view class="clock">
			<FlipClock></FlipClock>
		</view>
		<view class="date_week_time">
			<view class=""><text>{{ date.yyyy + '-'+ date.mm + '-'+ date.dd }}</text></view>
			<view class=""><text>星期{{ date.week }}</text></view>
		</view>
		<view class="Temperature_humidity">
			<view class="">
				<text>温度：</text>
				<text>7</text>
				<text>&#8451</text>
			</view>
			<view class="">
				<text>湿度：</text>
				<text>15</text>
				<text>%</text>
			</view>
		</view>
	<view class="doMore" >
		<view class="">
			<uni-link :href="href" text="操作" class="hreftext" style="text-decoration: none;"></uni-link>
		</view>		
	</view>
	</view>
</template>

<script>
	import FlipClock from '../../components/uni-clock/FlipClock.vue'
	import uCharts from '@/components/u-charts/u-charts.js';
	
		var _self;
		var canvaRing=null;
		var canvaColumn=null;
		var Url = 'http://192.168.9.228:30082';
		// var Url = 'http://41.229.37.14:8088';
	export default {
		data() {
			return {
				serverData:'',
				 timer:null,
				// 柜子数量
				CabinetCount :'',
				// 公共货位
				PublicPoistionCount:'',
				// 装备数量
				stocknamenum:'',
				// 库位总量
				PoistionCount :'',
				// 出入库记录
				realData:[],
				// 月度库存物资分类对比
				AppModelHelper:[],
				list:[],
				//操作跳转地址
				href: 'http://192.168.9.228:30081/Home/Login',
				// href: 'http://41.229.37.14:8080/Home/Login',
				Public_equipmentIsshow:false,
				Single_equipmentIsshow:false,
				Emergency_equipmentIsshow:false,
				//公共装备列表
				PublicData:[],
				//单警装备列表
				SingleData:[],
				//应急装备列表
				EmergencyData:[],
				equipmentTitalNum:"",           
				locationTitalNum:"",           
				autoplay: true,
				interval: 1000,
				duration: 500,
			
			
				    
			}
		},
		
		onLoad() {
			this.getList();
				},
				
		 methods: {
			 getList(){
				 uni.clearStorage();
				 console.log("测试")
				 uni.request({
				 	url: Url+'/api/ToolStore/GetResrchRespone',
				 	method: 'GET',
				 	data: {},
				 	success: res => {
						// console.log(res)
						// alert("连接成功");
				 		this.AppModelHelper = res.data.data.AppModelHelper;
				 		
				 		// console.log(this.AppModelHelper);
				 		this.realData = res.data.data.WareInOutrecords;
						
						
				 		this.PublicPoistionCount = res.data.data.PublicPoistionCount;
				 		this.CabinetCount = res.data.data.CabinetCount;
				 		this.stocknamenum = res.data.data.stocknamenum;
				 		let s = this.stocknamenum.toString().padStart(9,'0');
				 		var name = s.replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1,");
				 		var nameArray=new Array([name.length]);
				 		for(var i = 0;i<name.length;i++){
				 		nameArray[i]=name.charAt(i);
				 		} 
				 		this.equipmentTitalNum = nameArray;
				 		
				 		this.PoistionCount = res.data.data.PoistionCount;
				 		 let s1 = this.PoistionCount.toString().padStart(9,'0');
				 		 var name1 = s1.replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1,");
				 		 var nameArray1=new Array([name1.length]);
				 		 for(var i = 0;i<name1.length;i++){
				 		 nameArray1[i]=name1.charAt(i);
				 		 } 
				 		this.locationTitalNum = nameArray1;
				 		this.showRing('canvasRing',this.AppModelHelper)
				 	},
				 	fail: () => {
				 		// alert(Error);
				 	},
				 	complete: () => {},
				 	
				 });
				 
				 _self = this;
				 // this.showRing('canvasRing',this.AppModelHelper)
				
				 this.getClientTime();
				
				 this.getServerData();
				
			 },
			 getServerData(){
			 	uni.request({
			 		url:  Url+'/api/ToolStore/GetdocumentsRecord',
			 		data:{
			 		},
			 		success: function(res) {
			 			// console.log(res.data)
			 			//下面这个根据需要保存后台数据，我是为了模拟更新柱状图，所以存下来了
			 			_self.serverData=res.data;
			 			let Column={categories:[],series:[]};
			 			//这里我后台返回的是数组，所以用等于，如果您后台返回的是单条数据，需要push进去
			 			Column.categories=res.data.data.weekday;
			 			 // console.log(Column.categories)
			 			Column.series=res.data.data.series;
			 			 // console.log(Column.series)
			 			_self.showColumn("canvasColumn",Column);
			 		},
			 		fail: () => {
			 			_self.tips="网络错误，请检查合法域名";
			 		},
			 	});
			 },
			 showColumn(canvasId,chartData){
			 	canvaColumn=new uCharts({
			 		$this:this,
			 			canvasId: canvasId,
			 			type: 'column',
			 			legend:{
							show:true,
							fontColor:'#ffffff'
							},
			 			fontSize:11,
			 			background:'#FFFFFF',
			 			pixelRatio:1,
			 			// animation: true,
			 			categories: chartData.categories,
			 			series: chartData.series,
			 			xAxis: {
			 				disableGrid:true,
							fontSize:8
			 			},
			 			yAxis: {
							
							// data:{
							// 	fontSize:5
							// },
			 				 // disabled:true
							disableGrid:true,
							 // max:50
			 			},
						
			 			// dataLabel: true,
			 			width: 300,
			 			height: 190,
			 			extra: {
			 				column: {
			 					type:'group',
			 					width: 20
			 				}
			 			  }
			 	});
			 	
			 },
			getClientTime () {
			      const date = new Date()
			      const weeks = [ '日', '一', '二', '三', '四', '五', '六']
			      const that = this
			      this.date = {
			        yyyy: date.getFullYear(),
			        mm: date.getMonth() + 1,
			        dd: date.getDate(),
			       
			        week: weeks[ date.getDay() ]
			    }
			
			      if (this.date.mm < 10) {
			        this.date.mm = '0' + this.date.mm
			      }
			      if (this.date.dd < 10) {
			        this.date.dd = '0' + this.date.dd
			      }
			      
			setTimeout(() => {
			        that.getClientTime()
			      }, 1000)
			    },
			 
			 //点击显示详情方法
			 Public_equipment_click(e){
				 var id = e.currentTarget.dataset.id;
				 console.log(id)
				 this.Public_equipmentIsshow = !this.Public_equipmentIsshow;
				 uni.request({
				 	url: Url+'/api/ToolStore/GetStockNameTypeList?NameType='+id,
				 	method: 'GET',
				 	data: {},
				 	success: res => {
				 		// console.log(res);
						this.PublicData = res.data.data;
				 		// console.log(this.PublicData);
				 	},
				 	fail: () => {},
				 	complete: () => {}
				 });
			 },
			 Single_equipment_click(e){
				 var id = e.currentTarget.dataset.id;
				 // console.log(id)
			 	this.Single_equipmentIsshow = !this.Single_equipmentIsshow;
				uni.request({
					url: Url+'/api/ToolStore/GetStockNameTypeList?NameType='+id,
					method: 'GET',
					data: {},
					success: res => {
						// console.log(res);
						this.SingleData = res.data.data;
						// console.log(this.PublicData);
					},
					fail: () => {},
					complete: () => {}
				});
			 },
			 Emergency_equipment_click(e){
				 var id = e.currentTarget.dataset.id;
				 // console.log(id)
			 	this.Emergency_equipmentIsshow = !this.Emergency_equipmentIsshow;
				uni.request({
					url: Url+'/api/ToolStore/GetStockNameTypeList?NameType='+id,
					method: 'GET',
					data: {},
					success: res => {
						 // console.log(res);
						this.EmergencyData = res.data.data;
						 // console.log(this.PublicData);
					},
					fail: () => {},
					complete: () => {}
				});
			 },
			
			
			 //信息滚动相关操作
		        changeIndicatorDots(e) {
		            this.indicatorDots = !this.indicatorDots
		        },
		        changeAutoplay(e) {
		            this.autoplay = !this.autoplay
		        },
		        intervalChange(e) {
		            this.interval = e.target.value
		        },
		        durationChange(e) {
		            this.duration = e.target.value
		        },
				
				showRing(canvasId,AppModelHelper) {
								var chartData = {
									series :AppModelHelper,
								};
								canvaRing = new uCharts({
									$this: _self,
									canvasId: canvasId,
									type: 'ring',
									fontSize: 15,
									textColor:'#ffffff',
									legend:{
										show:true,
									fontColor:'#ffffff',
									},
									extra: {
										pie: {
											offsetAngle: -45,
											ringWidth: 30,
											labelWidth: 10,
											
										}
									},
									background: '#051A45',   // 中间圆的背景颜色	
									series: chartData.series,
									animation: false,     // 动画  画个圆的那种
									width: 300,
									height: 190,
									disablePieStroke: true,
									dataLabel: true,    //  图上红圈圈出来的线  true 显示
								});
							},
							

				
		    },
			//定时刷新数据 30秒
			mounted() {
				this.timer = setInterval(()=>{
					this.getList();
				},1000*30)
			},
			components: {
			  FlipClock
			},
			beforeDestroy(){
			   clearInterval(this.timer);        
			   this.timer = null;
			}
		
	}

</script>

<style scoped>
	*{
		padding: 0;
		margin: 0;
		box-sizing: border-box;
		list-style: none;
		font-size: 14px;
		color: white;
	}
	
	 
		.qiun-padding {
			padding: 2%;
			width: 96%;
		}
	 
		.qiun-wrap {
			display: flex;
			flex-wrap: wrap;
		}
	 
		.qiun-rows {
			display: flex;
			/* flex-direction: row !important; */
		}
	
	 
		.qiun-charts {
			width: 506px;
			height: 200px;
			margin-left: 20px;
		}
	 
		 .charts , .charts1{
			width: 500px;
			height: 200px;
		/* background-color: #FFFFFF; */
		} 

	.container {
		width: 1280px;
		height: 720px;
		display: flex;
		align-items: center;
		justify-content: space-around;
		overflow: auto;
		background-image: url(../../static/bg.png);
		background-repeat: no-repeat;
		background-size: 100% 100%;
		padding-top: 64px;
	}
	.left .title image,.right .title2 image,
	.Public_equipment .title2 image,
	.Single_equipment .title2 image,
	.Emergency_equipment .title2 image{
		width: 38px;
		height: 12px;
	}
	/deep/ .left .title ,.right .title2{
		padding-top: 10px;   
		color: white;
		font-size: 14px;
	}
	.left .title text,.right .title2 text{
		 padding: 0 13px;
	}
	.left .left_top{
		
		/* padding:19px 20px 0 20px; */
		
		/* margin-left: 38px; */
		height: 166.66px;
		width: 364px;
		background-image: url(../../static/1.png);
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}
	.left_top_info{
		    margin-top: 10px;
			text-align: center;
	}
	.left_top_num{
		text-align: center;
	 margin-top: 30px;
			
	}
	/deep/ .left_top_num text{
		display: inline-block;
		width: 23px;
		height: 44px;
		font-size: 40px;
		font-family: DINAlternate-Bold, DINAlternate;
		font-weight: bold;
		color: #7AF7F8;
		line-height: 47px;
		background-color: #062C5B;
		margin: 0 5px;
		

	}
	.activeText{
		background-color: transparent !important;
		width: 5px !important;
	}
	.right .title2{
		margin-left: 170px;
	}
	.left .left_center{
		/* padding:19px 20px 0 20px; */
		/* margin-left: 38px; */
		margin-top: 10px;
		height: 218.666px;
		width: 364px;
		background-image: url(../../static/2.png);
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}
	.left .left_bottom{
		/* padding:19px 20px 0 20px; */
		/* margin-left: 38px; */
		margin-top: 10px;
		height: 218.666px;
		width: 364px;
		background-image: url(../../static/3.png);
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}
	.center{
		width: 393.333px;
		height: 373.333px;
		background-image: url(../../static/center.png);
		background-repeat: no-repeat;
		background-size: 100% 100%;
	
	}
	.right{
		width: 364px;
	}
	.right_top{
		/* padding:19px 20px 0 20px; */
		width: 364px;
		height: 257.33px;
		background-image: url(../../static/4.png);
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}
	.right_bottom{
		/* padding:19px 20px 0 20px; */
		margin-top: 8px;
		width: 364px;
		height: 362.66px;
		background-image: url(../../static/5.png);
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}
	/deep/ .right .right_progressbar_auto text{
		color: #465C83;
	}
	.right .progress-box{
		width: 70%; 
		position: relative;
		left: 80px;
		top: -20px;
	}
	.right_progressbar{
		padding-top: 30px;
		padding-left: 25px;
	}
	.right_bottom_list_ul .right_bottom_list_ul_li .right_bottom_list_swiper{
		display: flex;
		padding: 10px 0;
	}
	.right_bottom_list ul:first-child{
		margin-top: 10px;
		background-color: #164275;
		padding: 5px 10px;
	}
	/deep/ .right_bottom_list ul .right_bottom_list_swiper text{
		font-size: 14px;
		color: #7797C3;
	}
	.right_bottom_list ul li{
		display: flex;
	}
	.right_bottom_list ul li text,.right_bottom_list_ul_li .right_bottom_list_swiper text{
		flex: 2;
	}
	.right_bottom_list ul li text:first-child,.right_bottom_list_ul_li .right_bottom_list_swiper text:first-child{
		flex: 6;
	}
	.right_bottom_list ul li text:last-child,.right_bottom_list_ul_li .right_bottom_list_swiper text:last-child{
		flex: 3;
	}
	.Public_equipment,.Single_equipment,.Emergency_equipment{
		position: absolute;
		width: 364px;
		height: 636px;
		
		
	}
	.Public_equipment,.Single_equipment{
		    left: 890px;
		    top: 78px;
		
		background-image: url(../../static/6.png);
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}
	.Public_equipment .title2,.Single_equipment .title2{
		margin-left: 220px;
	}
	.Public_equipment_list,.Single_equipment_list,.Emergency_equipment_list {
		padding: 0 20px;
		padding-top: 20px;
		
	}
	/deep/ .Public_equipment_list ul:first-child li,.Single_equipment ul:first-child li,.Emergency_equipment ul:first-child li{
		background-color: #164275;
		padding: 5px 0;
		
	}
	/deep/ .Public_equipment_list ul li text,.Single_equipment_list ul li text,.Emergency_equipment_list ul li text{
		font-size: 14px;
		color: #C2D9FF;
		display: inline-block;
		width: 100px;
		text-align: center;
	}
	/deep/ .Public_equipment_list ul li text:first-child,.Single_equipment_list ul li text:first-child,.Emergency_equipment_list ul li text:first-child{
		width: 200px;
		text-align: left;
		padding-left: 5px;
	}
	.Public_equipment_list ul li,.Single_equipment_list ul li,.Emergency_equipment ul li{
		display: flex;
		justify-content: space-around;
	}
	.Public_equipment_list_info li{
		padding: 10px 0;
	}
	.Public_equipment_list_info{
		height:880px ;
		overflow-y: auto;
	}
	.Emergency_equipment{
		    left: 26px;
		    top: 80px;
		
		background-image: url(../../static/7.png);
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}
	.Public_equipment_click{
		position: absolute;
		top: 235px;
		left: 716px;
		width: 93px;
		height: 91px;
		
	}
	.Single_equipment_click{
		position: absolute;
		top: 467px;
		left: 706px;
		width: 93px;
		height: 91px;
		
	}
	.Emergency_equipment_click{
		position: absolute;
		top: 343px;
		left: 445px;
		width: 93px;
		height: 91px;
		
	}
	.clock{
		width:450px;
		height: 95px;
		position: absolute;
		left: 410px;
		top: 80px;
	}
	.date_week_time{
		width: 200px;
		height: 96px;
		position: absolute;
		left: 400px;
		top: 150px;
		text-align: center;
	
			
	}
	.date_week_time text{
		font-size: 23px !important;
		color: #7AF7F8;
	}
	.doMore{
		width: 100px;
		height: 40px;
		position: absolute;
		left: 715px;
		top: 640px;
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: #062C5B;
		border-radius: 35px;
	}
	.hreftext{
		font-size: 25px !important;
		text-decoration: none;
		color: #7AF7F8 !important;
	}
	.doMore view text span {
		font-size: 30px !important;
		text-decoration: none;
		background-color: #062C5B;
		border-radius: 10px;
		padding: 5px 20px; 
		
	}
	.Temperature_humidity{
		  width: 296px;
		      height: 52px;
		      position: absolute;
		      left: 581px;
		      top: 151px;
		    display: flex;
		    justify-content: space-around;
		   
		    align-items: center;
	}
	.Temperature_humidity text{
		font-size: 25px;
		color: #7AF7F8 ;
	}
	.Temperature_humidity view text:first-child{
		/* color: #ffffff; */
		
	}
</style>
