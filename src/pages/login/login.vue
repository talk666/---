<template>
  <div>
    <div class="header">
      <div v-if="isLogin">
        <div class="header-title">请登录</div>
        <div class="header-info">Please Login Your Account</div>
      </div>
      <div v-else>
        <div class="header-title">请注册</div>
        <div class="header-info">Please Register Your Account</div>
      </div>
    </div>

    <div class="body">
      <div class="login-form">
        <van-cell-group>
          <van-field :value="username" input-align="left" required clearable  
              icon="question-o" placeholder="请输入用户名" @change="On_Usernameunfocus_Change"/>
          <van-field :value="password" input-align="left" type="password" 
              placeholder="请输入密码"  @change="On_Passwordunfocus_Change"/>
          <div v-if="!isLogin">
            <van-field :value="phonenum" input-align="left" required clearable
              placeholder="请输入绑定手机号" @change="On_Phonenum_Change"/>
          </div>
        </van-cell-group>
      </div>

      <div>
        <div v-if="isLogin">
          <van-button type="primary" slot="button" round block color="#3d7ef9" @click="OnClick_Login_Register">登陆</van-button>
        </div>
        <div v-else>
          <van-button type="primary" slot="button" round block color="#3d7ef9" @click="OnClick_Login_Register">注册</van-button>
        </div>
      </div>
      <div class="other-option">
        <span  @click="OnOptionclick" >{{isLogin?"账号注册":"去登录"}}</span>
        <span style="margin: 0 30px">|</span>
        <span>忘记密码</span>
      </div>

      <div class="copyright-wrapper">
        <div class="copyright">
          <span>Glx&Wxy</span>
        </div>
      </div>

      <van-toast id="van-toast" />
    </div>
  </div>
</template>

<script>
import Toast from "vant-weapp/dist/toast/toast"

export default {

  data () {
    return {
     username:'',
     password:'',
     phonenum:'',
     isLogin:true
    }
  },
  methods: {

    OnClick_Login_Register(event){
      var that = this

      Toast.loading({
        duration:0, //持续展示Toast 展示在最上层
        forbidClick:true, //操作中是否可以操作界面
        message:that.isLogin?'登陆中...':'注册中...'
      })

      //模拟延时
      setTimeout(()=>{
        if(that.isLogin){
           wx.getStorage({
            key: "user",
            data:{
              username:that.username,
              password:that.password,
              contect:that.phonenum
            },
            success(res){
              console.log(res.data);
              if(that.username == "glxwxy"){//if(that.username == res.data.username)  更改为固定账号登陆
                if(that.password == "552211"){//if(that.password == res.data.password){
                  Toast.success("登陆成功!^_^");
                  //500ms后跳转到首页
                  setTimeout(()=>{
                    wx.switchTab({
                      url: '/pages/index/main',
                      success: (result)=>{
                        
                      },
                      fail: ()=>{},
                      complete: ()=>{}
                    });
                  },500);
                }else{
                  Toast.fail("密码错误");
                }
              }else{
                Toast.fail("账号错误");
              }
            },
            fail(res){
              console.log(res);
              console.log("登陆失败");
              Toast.fail("登陆失败");
            }
          });
        }else{
          //注册
          wx.setStorage({
            key: "user",
            data:{
              username:that.username,
              password:that.password,
              contect:that.phonenum
            },
            success(res){
              console.log(res);
              console.log("注册成功");
              Toast.success("注册成功!^_^");
            },
            fail(res){
              console.log(res);
              console.log("注册失败");
              Toast.success("注册失败!=_=");
            }
          });
        }
      },1000) //箭头函数
    },
    OnClick_Register(event){
      var that =this;
      
    },
    On_Usernameunfocus_Change(event){
      var that = this
      console.log(event)
      console.log("username = " , event.mp.detail)
      that.username = event.mp.detail
    },
    On_Passwordunfocus_Change(event){
      var that = this
      console.log(event)
      console.log("password = " , event.mp.detail)
      that.password = event.mp.detail
    },
    OnOptionclick(event){
      var that = this;
      that.isLogin = !that.isLogin
      console.log(`当前处于${that.isLogin?"登陆":"注册"}状态`)
    },
    On_Phonenum_Change(event){
      var that = this;
    }
  }

}
</script>

<style lang="scss" scoped>
.header{
  height: 100px;
  padding: 50px 0; //上下拉伸25 左右为0
  background-color: #3d7ef9;
 

  .header-title{
    color: rgb(253, 253, 253);//字体颜色
    margin-left: 20px;
    font-size:28px;
    font-weight: 500;
  }
  .header-info{
    color: rgb(253, 253, 253);
    margin-left: 20px;
    font-size: 14px;
  }
  }

.body{
  padding: 20px;
  margin-top: -20px; //往上移 整数下     顶部圆角逻辑 往上偏移 圆形化 颜色
  border-radius: 15px 15px 0 0;
  background-color: rgb(255, 255, 255);
  .login-form{
    margin-bottom: 30px;

  }

  .other-option{
    display: flex;
    justify-content: center;
    margin-top: 20px;
    color: rgb(202, 208, 208);
  }
  
  .copyright-wrapper{
    display: flex;
    justify-content:center;
    
    .copyright{
      position: fixed;
      color: rgb(237, 235, 233);
      bottom: 30px;
    }
  }
}

</style>
