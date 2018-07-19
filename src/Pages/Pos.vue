<template>
    <div class="pos">
        <div>
            <el-row>
                <el-col :span="7" class="pos-order" id="order-list">

                    <el-tabs>
                        <el-tab-pane label="点餐">
                            <el-table :data="tableData" border style="width: 100%"  >

                                <el-table-column prop="goodsName" label="商品"></el-table-column>
                                <el-table-column prop="count" label="量" width="50"></el-table-column>
                                <el-table-column prop="price" label="金额" width="70"></el-table-column>

                                <el-table-column  label="操作" width="100" fixed="right">
                                    <template slot-scope="scope">
                                        <el-button type="text" size="small"  @click="deleteSingleGoods(scope.row)">删除</el-button>
                                        <el-button type="text" size="small" @click=" addOrderList (scope.row)">增加</el-button>

                                    </template>
                                </el-table-column>
                            </el-table>

                            <div class="totalDiv">
                                <small>数量：</small>
                                <strong>{{totalCount}}</strong> &nbsp;&nbsp;&nbsp;&nbsp;
                                <small>总计：</small>
                                <strong>{{totalMoney}}</strong> 元
                            </div>

                            <div class="order-btn">

                                <el-button type="warning">挂单</el-button>
                                <el-button type="danger" @click="deleteAllGoods">删除</el-button>
                                <el-button type="success" @click="payMoney"> 结账</el-button>

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
                        <div class="often-goods-list" >
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

                                <ul class="cookList">
                                    <li v-for="goods in goodsType1"   @click="addOrderList(goods)">
                                        <span class="foodImg"><img :src="goods.goodsImg"  width="100%" /></span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">￥{{goods.price}}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane >

                            <el-tab-pane   label="零食">
                                <ul class="cookList">
                                    <li v-for="goods in goodsType2"  @click="addOrderList(goods)">
                                        <span class="foodImg"><img :src="goods.goodsImg"  width=100% /></span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">￥{{goods.price}}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>


                            <el-tab-pane  label="饮料">
                                <ul class="cookList">
                                    <li v-for="goods in goodsType3"  @click="addOrderList(goods)">
                                        <span class="foodImg"><img  :src="goods.goodsImg"   width="100%" /></span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">{{goods.price}}</span>
                                    </li>
                                </ul>
                            </el-tab-pane>

                            <el-tab-pane  label="套餐">
                                <ul class="cookList">
                                    <li v-for="goods in goodsType4" @click="addOrderList(goods)">
                                        <span class="foodImg"><img  :src="goods.goodsImg"   width="100%" /></span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">{{goods.price}}</span>
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
    import axios from 'axios';
    export default {
        name: 'Pos',
        mounted: function () {

            // 设置高度
            var orderHeight = document.body.clientHeight;
            document.getElementById("order-list").style.height = orderHeight + 'px';
        },

        //从API获取数据
        created(){
            //获取常用商品列表
            axios.get('http://jspang.com/DemoApi/oftenGoods.php')
                .then(  response =>{
                         console.log(response)
                         this.oftenGoods = response.data;
                    })

                .catch( error => {
                    console.log('网络错误，不能访问');
                    console.log(response);

                })

            //获取分类商品的列表
            axios.get('http://jspang.com/DemoApi/typeGoods.php')
                .then( response =>{
                    this.goodsType1 = response.data[0];
                    this.goodsType2 = response.data[1];
                    this.goodsType3 = response.data[2];
                    this.goodsType4 = response.data[3];
                })

                .catch( error =>{
                    console.log(response);
                    alert('网络错误，不能访问')
                })
        },


        data() {
            return {
                tableData: [], //订单列表的值
                oftenGoods: [],  //常用商品
                goodsType1: [],
                goodsType2: [],
                goodsType3: [],
                goodsType4: [],

                totalMoney: 0, //订单总价格
                totalCount: 0  //订单商品总数量
            }
        },

        methods:{
            //添加订单列表

            addOrderList (goods){

                this.totalCount = 0;
                this.totalMoney = 0;

                let isHave = false;

             //首先判断该商品是否存在左边列表中
               for( let i=0; i<this.tableData.length; i++){
                   if(this.tableData[i].goodsId == goods.goodsId){
                       isHave= true;
                   }
               }

               if(isHave){

                   //如果存在，直接添加数量
                   let arr= this.tableData.filter(o => o.goodsId == goods.goodsId)
                   arr[0].count++;

               }else{

                   //如果列表中不存在
                   let newGoods ={
                       goodsId:  goods.goodsId,
                       price:    goods.price,
                       goodsName: goods.goodsName,
                       count: 1
                   }
                   this.tableData.push(newGoods)
               }

               this.getAllMoney();

            },

            //删除左侧所有商品
            deleteAllGoods(){
                this.tableData=[];
                this.totalCount = 0;
                this.totalMoney = 0;
            },

            //删除左侧单个商品
            deleteSingleGoods(goods){
                this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
                this.getAllMoney();
            },

            //计算整体数量和金额

            getAllMoney(){
                this.totalMoney = 0;
                this.totalCount = 0;

                if(this.tableData){
                    this.tableData.forEach((ele) =>{
                        this.totalCount += ele.count;
                        this.totalMoney = this.totalMoney + (ele.price * ele.count);
                    })
                }

            },

            //模拟结账
            payMoney(){
                if (this.totalCount !=0 ){
                    this.totalCount = 0;
                    this.totalMoney = 0;
                    this.tableData = [];
                    console.log('付款成功')
                }else{
                    console.log('失败')
                }
            }



        }






    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .pos {
        font-size: 12px;
    }
    .pos-order {
        background-color: #F9FAFC;
        border-right: 1px solid #C0CCDA;
    }
    .order-btn {
        margin-top: 10px;
        text-align: center;
    }
    .title {
        height: 20px;
        border-bottom: 1px solid #D3DCE6;
        background-color: #F9FAFC;
        padding: 10px;
    }
    .often-goods-list ul li {
        list-style: none;
        float: left;
        border: 1px solid #E5E9F2;
        padding: 10px;
        margin: 5px;
        background-color: #fff;
        cursor: pointer;
    }
    .goods-type {
        clear: both;
    }
    .o-price {
        color: #58B7FF;
    }
    .often-goods-list {
        border-bottom: 1px solid #C0CCDA;
        height: auto;
        overflow: hidden;
        padding-bottom: 10px;
        background-color: #F9FAFC;
    }
    .cookList li {
        list-style: none;
        width: 23%;
        border: 1px solid #E5E9F2;
        height: auto;
        overflow: hidden;
        background-color: #fff;
        padding: 2px;
        float: left;
        margin: 2px;
        cursor: pointer;
    }
    .cookList li span {
        display: block;
        float: left;
    }
    .foodImg {
        width: 40%;
    }
    .foodName {
        font-size: 18px;
        padding-left: 10px;
        color: brown;
    }
    .foodPrice {
        font-size: 16px;
        padding-left: 10px;
        padding-top: 10px;
    }
    .totalDiv {
        height: auto;
        overflow: hidden;
        text-align: right;
        font-size: 16px;
        background-color: #fff;
        border-bottom: 1px solid #E5E9F2;
        padding: 10px;
    }
</style>