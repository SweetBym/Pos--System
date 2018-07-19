<template>
    <div class="left-nav">
       <ul>
           <li>
               <i class="icon iconfont icon-shouyin1"></i>
               <div>收银</div>
           </li>

           <li>
               <i class="icon iconfont icon-dianpu"></i>
               <div>店铺</div>
           </li>

           <li>
               <i class="icon iconfont icon-huiyuan"></i>
               <div>会员</div>
           </li>

           <li>
               <i class="icon iconfont icon-shangpin"></i>
               <div>商品</div>
           </li>

           <li>
               <i class="icon iconfont icon-tongji"></i>
               <div>统计</div>
           </li>
       </ul>
    </div>
</template>

<script>
    export default {
        name: "leftNav",
        data (){
            return{

            }
        }
    }

</script>

<style scoped>
   .left-nav{
       float: left;
       color: #fff;
       font-size: 12px;
       height: 100%;
       background-color:  #1D8ce0;
       width: 5%;

   }

    .iconfont{
        font-size: 20px;
    }

    .left-nav ul{
        margin: 0;
        padding: 0;
    }

    .left-nav li{
        list-style: none;
        text-align: center;
        padding: 20px;
        border-bottom: 1px solid #20a0ff;
    }
</style>