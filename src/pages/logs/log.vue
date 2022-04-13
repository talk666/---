<template>
<div>
  <div class="We">
    <div class="Title">
      <div>
        <img class="image" src="/static/images/love.jpg">
      </div>
    </div>

    <div class="List">
      <div class="body">
        <div class="body-header" @click="Onclick_one">
          <span>我们在一起已经:</span>
          <span class="num">
            <span class="days_num">1254</span>
            <span class="days">days</span>
          </span>
        </div>
        <div class="body-text">起始日:2019-9-20</div> 
      </div>

      <div class="body">
        <div class="body-header" @click="Onclick_two">
          <span>距离 宝宝 生日还有:</span>
          <span class="num">
            <span class="days_num">48</span>
            <span class="days">days</span>
          </span>
        </div>
        <div class="body-text">起始日:1996-07-19</div>
      </div>

      <div class="body">
        <div class="body-header" @click="Onclick_third">
          <span>距离周年纪念日:</span>
          <span class="num">
            <span class="days_num">1254</span>
            <span class="days">days</span>
          </span>
        </div>
        <div class="body-text">起始日:2019-9-20</div>
      </div>

      <van-toast id="van-toast" />
    </div>
  </div>
</div>
</template>

<script>
import Toast from "vant-weapp/dist/toast/toast"

export default {
 
  data () {
    return {
    }
  },

  methods: {
    Onclick_one(event){
      var that = this

      Toast.loading({
        duration:0, //持续展示Toast 展示在最上层
        forbidClick:true, //操作中是否可以操作界面
        message:"功能开发中"
      })

      setTimeout(()=>{
        Toast.clear()
      },1000)
    },
    Onclick_two(event){
            var that = this

      Toast.loading({
        duration:0, //持续展示Toast 展示在最上层
        forbidClick:true, //操作中是否可以操作界面
        message:"功能开发中,你咋还点"
      })

      setTimeout(()=>{
        Toast.clear()
      },1000)
    },
    Onclick_third(event){
            var that = this

      Toast.loading({
        duration:0, //持续展示Toast 展示在最上层
        forbidClick:true, //操作中是否可以操作界面
        message:"爱你呦"
      })

      setTimeout(()=>{
        Toast.clear()
      },1000)
    },

  }
}
</script>




<style lang="scss" scoped>
.We{
  
  .Title{
    padding: 50px 0; //上下拉伸25 左右为0
  }
  .List{
    padding-left: 10px;
    padding-right: 10px;

    .body{
      background-color: rgb(251, 236, 236);
      width: 100%;
      height: 60px;
      border-radius: 20px;
      box-shadow: #d6d6d6 10px 0px 10px;
      margin-top: 5px;/*上边距 */

      .body-header{
        font-size: 15px;
        padding-left: 20px;
        color: rgb(76, 64, 49);

        .num{
          padding-left: 110px;
          .days_num{
            font-size: 25px;
            color: rgb(217, 56, 155);
          }
          .days{
            color: rgba(132, 59, 59, 0.286);
          }
        }
        
      }
      .body-text{
        font-size: 10px;
        padding-left: 20px;
        color: rgba(72, 110, 166, 0.927);
      }
    }

  }
}
  
</style>
