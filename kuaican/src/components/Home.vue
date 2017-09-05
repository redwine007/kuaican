<template>
	<div class="pos">
		
		<leftNav></leftNav>
		<div class="main">
			<el-row>
				<el-col :span='7' id="listNav">
					<el-tabs>
						<el-tab-pane label="点餐">
							<el-table :data="tdata" border style="width: 100%;">
								<el-table-column prop="goodsName" label="商品名"></el-table-column>
								<el-table-column prop="count" label="量" width="60"></el-table-column>
								<el-table-column prop="price" label="金额" width="70"></el-table-column>
								<el-table-column label="操作" width="100" fixed="right">
									<template scope="scope">
										<el-button type="text" size="small" @click="deletList(scope.row)">删除</el-button>
										<el-button type="text" size="small" @click="addList(scope.row)">增加</el-button>
									</template>
								</el-table-column>
							</el-table>
							<div class="totalDiv">
                                <small>数量：</small>
                                <strong>{{totalCount}}</strong> &nbsp;&nbsp;&nbsp;&nbsp;
                                <small>总计：</small>
                                <strong>{{totalMoney}}</strong> 元
                            </div>
							<div class="Abtn">
								<el-button type="warning">挂单</el-button>
								<el-button type="danger" @click="deletAll">删除</el-button>
								<el-button type="success"@click="closeAll">结账</el-button>
							</div>
							
						</el-tab-pane>
						<el-tab-pane label="挂单">
							挂单
						</el-tab-pane>
						<el-tab-pane label="外卖">
							外卖
						</el-tab-pane>
					</el-tabs>
					
					
					
				</el-col>
				<el-col :span="17">
					<div class="hotList">
					    <div class="title">热门商品</div>
					    <div class="hotgoodList">
					        <ul>
					            <li v-for="x in hotgoods" @click="addList(x)">
					                <span>{{x.goodsName}}</span>
					                <span class="o-price">￥{{x.price}}元</span>
					            </li>
					        </ul>
					    </div>
					</div>
					
					<div class="goods-type"> 
					    <el-tabs type="border-card">
					        <el-tab-pane label="汉堡">
					           	<ul class="hbarList">
					           		<li v-for="n in type0Goods"  @click="addList(n)">
					           			<span class="foodImg"><img :src="n.goodsImg" width="100%"></span>
								        <span class="foodName">{{n.goodsName}}</span>
								        <span class="foodPrice">￥{{n.price}}元</span>
					           		</li>
					           	</ul>
					        </el-tab-pane>
					        <el-tab-pane label="小食">
					         	<ul class="hbarList">
					           		<li v-for="n in type1Goods"  @click="addList(n)">
					           			<span class="foodImg"><img :src="n.goodsImg" width="100%"></span>
								        <span class="foodName">{{n.goodsName}}</span>
								        <span class="foodPrice">￥{{n.price}}元</span>
					           		</li>
					           	</ul>
					        </el-tab-pane>
					        <el-tab-pane label="饮料">
					          	<ul class="hbarList">
					           		<li v-for="n in type2Goods"  @click="addList(n)">
					           			<span class="foodImg"><img :src="n.goodsImg" width="100%"></span>
								        <span class="foodName">{{n.goodsName}}</span>
								        <span class="foodPrice">￥{{n.price}}元</span>
					           		</li>
					           	</ul>
					        </el-tab-pane>
					        <el-tab-pane label="套餐">
					         	<ul class="hbarList">
					           		<li v-for="n in type3Goods"  @click="addList(n)">
					           			<span class="foodImg"><img :src="n.goodsImg" width="100%"></span>
								        <span class="foodName">{{n.goodsName}}</span>
								        <span class="foodPrice">￥{{n.price}}元</span>
					           		</li>
					           	</ul>
					        </el-tab-pane>
					 
					    </el-tabs>
					</div>
				</el-col>
			</el-row>
			
			<router-view></router-view>
		</div>
	</div>
	
</template>

<script>
	import leftNav from "./common/leftNav"
	import axios from 'axios'
	export default{
		name:"Pos",
		data(){
			return{
				tdata:[],
		        hotgoods:[],
		        type0Goods:[],
		        type1Goods:[],
		        type2Goods:[],
		        type3Goods:[],
		        totalMoney:0,
		        totalCount:0
			}
		},
		components:{
			leftNav
		},
		created:function(){
			axios.get('http://jspang.com/DemoApi/oftenGoods.php')
			.then(res=>{
				this.hotgoods = res.data
			})
			.catch(err=>{
				console.log("网路错误")
				
			});
			
			axios.get('http://jspang.com/DemoApi/typeGoods.php')
			.then(res=>{
				console.log(res)
				this.type0Goods = res.data[0];
				this.type1Goods = res.data[1];
				this.type2Goods = res.data[2];
				this.type3Goods = res.data[3];
			})
			.catch(err=>{
				console.log("网路错误")
				
			})
			
			
		},
		mounted:function(){
			let navHight = document.body.clientHeight;
			console.log(navHight);
			document.getElementById('listNav').style.height=navHight + 'px';			
		},
		methods:{
			addList(goods){
				//数量\金额清零
				this.totalMoney=0;
				this.totalCount=0;
				
				//商品是否已经存在于订单列表
				let isHave = false;
				for(let i=0;i<this.tdata.length;i++){
					if(this.tdata[i].goodsId == goods.goodsId){
						isHave = true;
					}
				};
				//根据判断的值进行业务逻辑
				
				if(isHave){
					//改变列表中商品的数量
					let arr = this.tdata.filter(o=>o.goodsId == goods.goodsId);
					arr[0].count++;
				}else{
					let newGoods={
						goodsId:goods.goodsId,
						goodsName:goods.goodsName,
						price:goods.price,
						count:1
					};
					this.tdata.push(newGoods)
					console.log(this.tdata)
				}
				this.getAllMoney()
				
			},
			deletList(goods){
				this.tdata = this.tdata.filter(o => o.goodsId != goods.goodsId);
				this.getAllMoney()
			},
			deletAll(){
				this.tdata= [],
				this.totalMoney=0;
				this.totalCount=0;
			},
			closeAll(){
				if(this.totalCount!=0){
					this.deletAll();
					this.$message({
						message:"结账成功！",
						type:'success'
						
					})
				}else{
					this.$message.error('请先选择商品！')
				}
			},		
			getAllMoney(){
				this.totalMoney=0;
				this.totalCount=0;
				this.tdata.forEach((element)=>{
					this.totalCount += element.count;
					this.totalMoney = this.totalMoney + (element.price*element.count);
				})
			}
		}
	}
</script>

<style>
	.pos{
		 font-family: 'Microsoft YaHei','Avenir', Helvetica, Arial, sans-serif;
		  -webkit-font-smoothing: antialiased;
		  -moz-osx-font-smoothing: grayscale;
		  text-align: left;
		  color: #2c3e50;
		   height:100%;
	}
	.main{
		  float:left;
		  width:95%; 
		  background-color: #EFF2F7;
		  height:100%;
		  overflow: auto;
		 
		}
		#listNav{
			background: #F9FAFC;
			overflow-x: hidden;
		}
		.Abtn{
			margin-top: 20px;
			text-align: center;
		}
	.hotList{
		width: 100%;
	}
	 .title{
	       height: 21px;
	       border-bottom:1px solid #D3DCE6;
	       background-color: #F9FAFC;
	       padding:10px;
	   }
	   .hotgoodList{
	   	 border-bottom: 1px solid #C0CCDA;
	    /*height: auto;*/
	    overflow: hidden;
	    padding-bottom: 20px;
	    background-color: #F9FAFC;
	   } 
	   .hotgoodList ul li{
	      float:left;
	      border-radius:5px;
	      border:1px solid #E5E9F2;
	      padding:10px;
	      margin:5px;
	      background-color:#fff;
	  
	   }
	  .o-price{
	      color:#58B7FF; 
	   }
	   .goods-type{
	   		clear: both;
	   }
		.hbarList li{
	       list-style: none;
	       width:23%;
	       border:1px solid #E5E9F2;
	       height: auot;
	       overflow: hidden;
	       background-color:#fff;
	       padding: 2px;
	       float:left;
	       margin: 2px;
	       cursor: pointer
	 
	   }
	   .hbarList li span{      
	        display: block;
	        float:left;
	   }
	   .hbarList li span:nth-child(2){
	   		width: 56%;
	   		overflow: hidden;
	   		text-overflow: ellipsis;
	   		white-space: nowrap;
	   }
	   .foodImg{
	       width: 40%;
	   }
	   .foodName{
	       font-size: 18px;
	       padding-left: 5px;
	       color:brown;
	 
	   }
	   .foodPrice{
	       font-size: 16px;
	       padding-left: 5px;
	       padding-top:10px;
	   }
	   .totalDiv {
		    height: auot;
		    overflow: hidden;
		    text-align: right;
		    font-size: 16px;
		    background-color: #fff;
		    border-bottom: 1px solid #E5E9F2;
		    padding: 10px;
		}
</style>