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
    var that = this;
    let year = []
    let month = []
    let day = []

    var i,j
    wx.getLocation({
      type: 'wgs84',
      success (res) {
          const latitude = res.latitude //维度
          const longitude = res.longitude //经度
          const key = "5a022a6bc3ac4d918dfcf2e117289aa6"
        //只用来获取一下网络时间
        wx.request({
        url: `https://devapi.qweather.com/v7/weather/now?location=${longitude},${latitude}&key=${key}`, //获取天气数据api接口 和风天气

        success (res) {
          for(i = 0; i < 4; i++){
            year[i] = parseInt(res.data.now.obsTime[i])
          }
          j = 0
          for(i = 0; i < 2; i++){
            j = i + 5
            month[i] = parseInt(res.data.now.obsTime[j])
          }
          j = 0
          for(i = 0; i < 2; i++){
            j = 8 + i
            day[i] = parseInt(res.data.now.obsTime[j])
          }
//当前时间
          var num_year = year[0]*1000+year[1]*100+year[2]*10+year[3]
          var num_month = month[0]*10+month[1]
          var num_day = day[0]*10+day[1]
//计算在一起的时间
          var together_days = (num_year - 2019)*365 + (num_month - 9)*30 + (num_day - 20)
//更新到主题
          that.Sum_days = together_days

//计算阳历生日
          var birthday_days = (num_month - 6)*30 + (num_day - 17)
          if(birthday_days < 0){//也就说生日在本年还没到
            that.Birth_days = (-birthday_days)
          }else{
            that.Birth_days = (365 - birthday_days)
          }
//计算周年纪念
          var  spcial_days= (num_month - 9)*30 + (num_day - 20)
          if(spcial_days < 0){//也就说生日在本年还没到
            that.Year_days = (-spcial_days)
          }else{
            that.Year_days = (365 - spcial_days)
          }
          }
        })

      }
    })
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
