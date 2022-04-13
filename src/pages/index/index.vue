<template>
  <div class="wrapper">
    <div  class="header-wrapper">
      <div class="header-title">
        <span>
          空气质量-{{airText}}
        </span>
        <span>
         {{area}}-{{city}}
        </span>
      </div>
      <div class="header-text">
        <span>
          {{Tmp}}℃
        </span>
        <span>
          {{weather}}
        </span>
      </div>
      <div class="header-advice">
        <span>
          {{weatherAdvice}}
        </span>
      </div>
    </div>
    
    <div  class="body-wrapper">
      <div class="body">
        <div class="data-wrapper">
          <div class="data">
            <img class="data-logo" src="/static/images/Tmp.png" />
            <div class="data-text">
              <div class="data-title">温度</div>
              <div class="data-value">{{Temp}}℃</div>
            </div>
          </div>

          <div class="data">
            <img class="data-logo" src="/static/images/shidu.png" />
            <div class="data-text">
              <div class="data-title">湿度</div>
              <div class="data-value">%{{Hum}}</div>
            </div>
          </div>
          
        </div>

        <div class="data-wrapper">
          <div class="data">
            <img class="data-logo" src="/static/images/light.png" />
            <div class="data-text">
              <div class="data-title">光照度</div>
              <div class="data-value">{{Light}}Lx</div>
            </div>
          </div>

          <div class="data">
            <img class="data-logo" src="/static/images/LED.png" />
            <div class="data-text">
              <div class="data-title">客厅灯</div>
              <div class="data-value">
                <switch @change="onLedChange" :checked = "LED" color="#3d7ef9"/>
              </div>
            </div>
          </div>
          
        </div>

        <div class="data-wrapper">
          <div class="data">
            <img class="data-logo" src="/static/images/beep.png" />
            <div class="data-text">
              <div class="data-title">报警器</div>
              <div class="data-value">
                <switch @change="onBeepChange" :checked = "Beep" color="#3d7ef9"/>
              </div>
            </div>
          </div>
          
        </div>
      </div>
    </div>
  </div>
</template>

<script>

const mqttUrl = "wxs://h0njatWd7OS.iot-as-mqtt.cn-shanghai.aliyuncs.com:443"
const iot = require('alibabacloud-iot-device-sdk');

import mqtt from "mqtt"; // 引入mqtt模块

export default {
  data () {
    return {
      client:{},
      Temp:0,//使用{{Temp}}绑定
      Hum:0,
      Light:0,
      LED:0,
      Beep:0,
      area:'请求中...', //地区
      city:'请求中...', //城市
      airText:'请求中...', //空气优良
      Tmp:0, //室外温度
      weather:'请求中...', //天气
      weatherAdvice:'请求中...'//建议
    }
  },

  components: {
  },

  methods: {
    //开关灯下发到阿里云上
    onLedChange(event){
      var that = this
      
      console.log(event.mp.detail.value)
      let sw = event.mp.detail.value
      //console.log(sw)
      that.LED = sw 
      if(sw){
        that.client.publish("/h0njatWd7OS/wx_program_dev/user/Cap_Control","{\"StatusLightSwitch\" : 1}",function (err){
        //that.client.publish("/h0njatWd7OS/Transfor_test/user/get","{\"StatusLightSwitch\" : 1}",function (err){
          if(!err){
              console.log("成功发送命令：开灯")
            }else{
              console.log("发送命令：开灯 失败!!!")
            }
        })
      }else{
        that.client.publish("/h0njatWd7OS/wx_program_dev/user/Cap_Control","{\"StatusLightSwitch\" : 0}",function (err){
        //that.client.publish("/h0njatWd7OS/Transfor_test/user/get","{\"StatusLightSwitch\" : 0}",function (err){
          if(!err){
              console.log("成功发送命令：关灯")
            }else{
              console.log("发送命令：关灯 失败!!!")
            }
        })
      }
    },

    //告警下发到阿里云上
    onBeepChange(event){
      var that = this
      
      console.log(event.mp.detail.value)
      let sw = event.mp.detail.value
      //console.log(sw)
      that.Beep = sw 
      if(sw){
        that.client.publish("/h0njatWd7OS/wx_program_dev/user/Cap_Control","{\"AlarmPromptSwitch\" : 1}",function (err){
        //that.client.publish("/h0njatWd7OS/Transfor_test/user/get","{\"StatusLightSwitch\" : 1}",function (err){
          if(!err){
              console.log("成功发送命令：报警")
            }else{
              console.log("发送命令：报警 失败!!!")
            }
        })
      }else{
        that.client.publish("/h0njatWd7OS/wx_program_dev/user/Cap_Control","{\"AlarmPromptSwitch\" : 0}",function (err){
        //that.client.publish("/h0njatWd7OS/Transfor_test/user/get","{\"StatusLightSwitch\" : 0}",function (err){
          if(!err){
              console.log("成功发送命令：关闭报警")
            }else{
              console.log("发送命令：关闭报警 失败!!!")
            }
        })
      }
    }
    
  },

  // created () {
  //   // let app = getApp()
  // }
  onShow() {
   /*阿里云官网demo
   console.log('正在连接...')
    
    const device = iot.device({
      productKey: 'h0njatWd7OS',
      deviceName: 'Transfor_test',
      deviceSecret: '4e87383055d5ecce649a1e517e24a643',
      brokerUrl: 'wxs://h0njatWd7OS.iot-as-mqtt.cn-shanghai.aliyuncs.com:443'
    });
    
    device.on('connect', () => {
      console.log('connect successfully!');
    })
    */

    let options={
      connectTimeout: 4000, // 超时时间
      clientId: 'FESA234FBDS24|securemode=3,signmethod=hmacsha1,timestamp=789|', // id
      username: 'wx_program_dev&h0njatWd7OS', // 用户名
      password: '8d1b73f0a0176d4c3173d3b05732c5575024fdf5', // 密码
      cleanSession: false,
      keepAlive: 300, // 心跳值，心跳值太大可能会连接不成功，这个参考文档
    };
    
    console.log('正在连接...')
    try {
      var that = this
      //that.client = connect(mqttUrl)
      that.client = mqtt.connect(mqttUrl, options)
    } catch (error) {
        console.log('mqtt连接失败: ', error)
      }
    that.client.on('connect', (e) => {
      console.log('连接 ALiYun MQTT Server 成功')
      //订阅Topic
      that.client.subscribe("/h0njatWd7OS/wx_program_dev/user/Cap_Control", function (err){
        if(!err){
          console.log("成功订阅设备上行数据Topic")
        }
      })
    })

    that.client.on('message', function (topic,message){
      console.log(topic)
      //console.log(message)
      //message 为十六进制buff字节流 所以需要进行转化
      let dataFromDev = {}
      //上行设备发送格式按照json格式进行数据传输 测试：{"StatusLightSwitch" : 0,"TargetTemperature":100.12}
      dataFromDev = JSON.parse(message)
      console.log(dataFromDev)
      //将阿里云下发的状态进行组件更新
      that.Temp = dataFromDev.TargetTemperature //从解析出来的json数据赋值到组件中数据
      that.Hum = dataFromDev.Hum
      that.Light = dataFromDev.Light
      that.LED = dataFromDev.StatusLightSwitch
      that.Beep = dataFromDev.Beep
    })

    wx.getLocation({
      type: 'wgs84',
      success (res) {
          const latitude = res.latitude //维度
          const longitude = res.longitude //经度
          const key = "5a022a6bc3ac4d918dfcf2e117289aa6"
        //网络请求获取空气情况
        wx.request({
        url: `https://devapi.qweather.com/v7/weather/now?location=${longitude},${latitude}&key=${key}`, //获取天气数据api接口 和风天气

        success (res) {
          console.log(res.data)
          that.weather = res.data.now.text
          that.Tmp = res.data.now.temp
          }
        })

        //网络请求 获取空气建议
        wx.request({
        url: `https://devapi.qweather.com/v7/indices/1d?type=1,2&location=${longitude},${latitude}&key=${key}`, //获取天气数据api接口 和风天气
        
        success (res) {
          console.log(res.data)
          that.weatherAdvice = res.data.daily[0].text
          }
        })

        //网络请求获取空气质量情况
        wx.request({
        url: `https://devapi.qweather.com/v7/air/now?&location=${longitude},${latitude}&key=${key}`, //获取天气数据api接口 和风天气

        success (res) {
          console.log(res.data)
          that.airText = res.data.now.category
          }
        })

        //网络请求 从风景接口获取城市信息
        wx.request({
        url: `https://geoapi.qweather.com/v2/poi/lookup?type=scenic&location=${longitude},${latitude}&key=${key}`, //获取天气数据api接口 和风天气
        success (res) {
          console.log(res.data)
          that.area = res.data.poi[0].adm1
          that.city = res.data.poi[0].adm2
          }
        })
      }
    })

  }
};
</script>

<style lang="scss" scoped>
.wrapper{
  padding: 15px;
    .header-wrapper{
      background-color: #3d7ef3;
      border-radius: 20px;
      color: #fff;
      box-shadow: #d6d6d6 0px 0px 5px;
      padding: 15px 30px;

    .header-title{
      display: flex;
      justify-content: space-between;
    }

    .header-text{
      font-size: 32px;
      font-weight: 400;
      display: flex;
      justify-content: space-between;
    }

    .header-advice{
      margin-top: 20px;
      font-size: 12px;
    }
  }

  .data-wrapper{
      margin-top: 20px;
      display: flex;
      justify-content: space-between;
      .data{
        background-color: #fff;
        width: 150px;
        height: 80px;
        border-radius: 20px;
        display: flex;
        justify-content: space-between;
        padding: 0 8px;
        box-shadow: #d6d6d6 0px 0px 10px;
      
      .data-logo{
        height: 36px;
        width: 36px;
        margin-top: 23px;/*上边距 */
        margin-left: 15px;
      }

      .data-text{
        margin-top: 12px;
        margin-right: 15px;
        .data-title{
          text-align: right;
        }
        color: #7f7f7f;

      .data-title{
        align-items: center;
      }
      .data-value{
        font-size: 23px;
        color: #d6d6d6;
      }
      }
    }
  }
}
</style>
