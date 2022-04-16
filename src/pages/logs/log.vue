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
            <span class="days_num">{{Sum_days}}</span>
            <span class="days">days</span>
          </span>
        </div>
        <div class="body-text">起始日:2019-9-20</div> 
      </div>

      <div class="body">
        <div class="body-header" @click="Onclick_two">
          <span>距离 宝宝 生日还有:</span>
          <span class="num">
            <span class="days_num">{{Birth_days}}</span>
            <span class="days">days</span>
          </span>
        </div>
        <div class="body-text">起始日:1996-07-19</div>
      </div>

      <div class="body">
        <div class="body-header" @click="Onclick_third">
          <span>距离周年纪念日:</span>
          <span class="num">
            <span class="days_num">{{Year_days}}</span>
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
      Year_days:1121,
      Birth_days:52,
      Sum_days:1547,
      now_time:20220417
    }
  },

  onShow() {
    var that = this
    that.Get_time_fordist("2019-09-20","1996-07-19")
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
    Get_time_fordist(Date_end,Birth){
      var that = this
      let date1 = new Date(Date_end);
      let date2 = new Date();
      let date3 = new Date(Birth)
  //获取在一起总时间
      date1 = new Date(date1.getFullYear(), date1.getMonth(), date1.getDate());
      date2 = new Date(date2.getFullYear(), date2.getMonth(), date2.getDate());
      //console.log("qqq",date2,"sdada",date1)
      const diff = date2.getTime() - date1.getTime(); //目标时间减去当前时间
      const diffDate = diff / (24 * 60 * 60 * 1000);  //计算当前时间与结束时间之间相差天数
      //console.log("dasdasda",diffDate)
      that.Sum_days = diffDate
  //获取周年纪念
      date1 = new Date(date2.getFullYear(), date1.getMonth(), date1.getDate());
      date2 = new Date(date2.getFullYear(), date2.getMonth(), date2.getDate());
      const diff_year = date2.getTime() - date1.getTime(); //目标时间减去当前时间
      var diffDate_year = diff_year / (24 * 60 * 60 * 1000);  //计算当前时间与结束时间之间相差天数
      //console.log("dasdasda",diffDate_year)
      if(diffDate_year < 0) diffDate_year = (-diffDate_year)
      that.Year_days = diffDate_year
  //获取生日倒计时
      date1 = new Date(date2.getFullYear(), date3.getMonth(), date3.getDate());
      date2 = new Date(date2.getFullYear(), date2.getMonth(), date2.getDate());
      //console.log("qqq",date2,"sdada",date1)
      const diff_birth = date2.getTime() - date1.getTime(); //目标时间减去当前时间
      var diffDate_birth = diff_birth / (24 * 60 * 60 * 1000);  //计算当前时间与结束时间之间相差天数
      //小于零说明本年生日还没到 大于0根据本年是闰还是平年计算下年生日时间
      that.Birth_days = diffDate_birth < 0?(-diffDate_birth):(diffDate_birth%4==0?(366-diffDate_birth):365-diffDate_birth)
    }
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
