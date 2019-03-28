<template>
  <div class="pos">
    <div>
       <el-row>
            <el-col :span='7'  id="order-list" class="tab">
                <el-tabs>
                      <el-tab-pane label="点餐">
                          <el-table :data="tableData" border  style="width: 100%" >
                            <el-table-column prop="goodsName" label="商品"  ></el-table-column>
                            <el-table-column prop="count" label="量" width="50"></el-table-column>
                            <el-table-column prop="price" label="金额" width="70"></el-table-column>
                            <el-table-column  label="操作" width="100" fixed="right">
                                <template scope="scope">
                                    <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                </template>
                            </el-table-column>
                           </el-table> 
                           <div class="totaldiv">
                             <small>数量：</small>{{totalCount}}  &nbsp;&nbsp;&nbsp;&nbsp;
                             <small>金额：</small>{{totalMoney}}元
                           </div>
                           <div class="div-btn">
                             <el-button type="warning" >挂单</el-button>
                             <el-button type="danger" @click="delAllGoods">删除</el-button>
                             <el-button type="success" @click="checkOut">结账</el-button> 
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
            
           
            <!--商品展示-->
            <el-col :span="17">
               <div class="often-goods">
                   <div class="title">常用商品</div>
                   <div class="often-goods-list">
                       <ul>
                           <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                               <span>{{goods.goodsName}}</span>
                               <span class="o-price">￥{{goods.price}}元</span>
                           </li>
                       </ul>
                   </div>
               </div>
               <div class="goods-type">
                   <el-tabs>
                       <el-tab-pane label="汉堡">
                  
                           <ul class='cookList'>
                               <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                               </li>
                           </ul>
                       </el-tab-pane>
                      <el-tab-pane label="小食">
            
                           <ul class='cookList'>
                               <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                               </li>
                           </ul>
                       </el-tab-pane>
                       <el-tab-pane label="饮料">
                    
                           <ul class='cookList'>
                               <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                               </li>
                           </ul>
                       </el-tab-pane>
                       <el-tab-pane label="套餐">
                  
                           <ul class='cookList'>
                               <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                               </li>
                           </ul>
                       </el-tab-pane>
                   </el-tabs>
               </div>
               
            </el-col>
        </el-row>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data(){
    return {
      tableData: [],
      oftenGoods:[
        {
                     goodsId:1,
                     goodsName:'香辣鸡腿堡',
                     price:18
                 }, {
                     goodsId:2,
                     goodsName:'田园鸡腿堡',
                     price:15
                 }, {
                     goodsId:3,
                     goodsName:'和风汉堡',
                     price:15
                 }, {
                     goodsId:4,
                     goodsName:'快乐全家桶',
                     price:80
                 }, {
                     goodsId:5,
                     goodsName:'脆皮炸鸡腿',
                     price:10
                 }, {
                     goodsId:6,
                     goodsName:'魔法鸡块',
                     price:20
                 }, {
                     goodsId:7,
                     goodsName:'可乐大杯',
                     price:10
                 }, {
                     goodsId:8,
                     goodsName:'雪顶咖啡',
                     price:18
                 }, {
                     goodsId:9,
                     goodsName:'大块鸡米花',
                     price:15
                 }, {
                     goodsId:20,
                     goodsName:'香脆鸡柳',
                     price:17
                 }
      ],
      type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[],
      totalCount:0,
      totalMoney:0
    }
  },
  created(){
      axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
      .then(response=>{
         console.log(response);
         this.oftenGoods=response.data;
      })
      .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
      })
      //读取分类商品列表
       axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
       .then(response=>{
          console.log(response);
          //this.oftenGoods=response.data;
          this.type0Goods=response.data[0];
          this.type1Goods=response.data[1];
          this.type2Goods=response.data[2];
          this.type3Goods=response.data[3];
       })
       .catch(error=>{
           console.log(error);
           alert('网络错误，不能访问');
       })
  },
  mounted:function(){
    let orderHeight=document.body.clientHeight;
    document.getElementById("order-list").style.height=orderHeight+'px';
  },
  methods:{
    addOrderList(goods){
      
      
      let tableData =this.tableData;
      let isHave =false;
      for (let i = 0; i < tableData.length; i++) {
        if (tableData[i].goodsId===goods.goodsId) {
           isHave =true;
        }
      }

      if (isHave) {
        let arr = tableData.filter(o =>o.goodsId == goods.goodsId);
        arr[0].count++;   
      }else{
        let res={goodsName: goods.goodsName,price: goods.price,count:1,goodsId:goods.goodsId};
        this.tableData.push(res)
      }

      //进行数量和价格的汇总计算
      // tableData.forEach((element) => {
      //     this.totalCount+=element.count;
      //     this.totalMoney=this.totalMoney+(element.price*element.count);   
      // });

      this.getAllMoney();
      
    },
    delAllGoods(){
      this.tableData=[];
      this.totalCount=0;
      this.totalMoney=0;

    },
    delSingleGoods(goods){
      this.tableData = this.tableData.filter(o =>o.goodsId != goods.goodsId);
      this.getAllMoney();
    },
    getAllMoney(){
      this.totalCount=0;
      this.totalMoney=0;
      let tableData =this.tableData;
      if(tableData){
          for (let i = 0; i < tableData.length; i++) {
            this.totalCount=this.totalCount+tableData[i].count
            this.totalMoney=this.totalMoney+(tableData[i].price*tableData[i].count)
          }
      }
    },
    checkOut(){
      if(this.totalCount!=0){
        this.tableData=[];
        this.totalCount=0;
        this.totalMoney=0;
        this.$message({
          message:'结算成功！',
          type:"success"
        })
      }else{
        this.$message.error('不能空结账！');
      }
    }
  }
}
</script>
<style scoped>
    .tab{
      background: #fff!important;
    }
    .div-btn{
      margin-top: 20px;
      text-align: center;
    }
    .title{
         height: 20px;
         border-bottom:1px solid #D3DCE6;
         background-color: #F9FAFC;
         padding:10px;
     }
     .often-goods-list ul li{
        list-style: none;
        float:left;
        border:1px solid #E5E9F2;
        padding:10px;
        margin:5px;
        background-color:#fff;
     }
     .o-price{
        color:#58B7FF; 
     }
     .cookList li{
          list-style: none;
          width:23%;
          border:1px solid #E5E9F2;
          height: auot;
          overflow: hidden;
          background-color:#fff;
          padding: 2px;
          float:left;
          margin: 2px;
      }
      .cookList li span{
           display: block;
           float:left;
      }
      .foodImg{
          width: 40%;
      }
      .foodName{
          font-size: 18px;
          padding-left: 10px;
          color:brown;
      }
      .foodPrice{
          font-size: 16px;
          padding-left: 10px;
          padding-top:10px;
      }
      .often-goods{
          overflow: hidden;
      }
      .totaldiv{
        background: #fff;
        padding: 10px;
        border-bottom: 1px solid #eee;
        text-align: center;
      }
</style>