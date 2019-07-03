<template>
    <div>
        <div id="top">
            <img @click="back()" id="img1" src="../../assets/back.png" alt="">
            <p id="login">重置密码</p>
        </div>
        <div id="center">
            <input v-model="username" id="ac" placeholder="账号" type="text">
            <input v-model="oldpassWord" id="se" placeholder="旧密码" type="text">
            <input v-model="newpassword" id="nse" placeholder="请输入新密码" type="text">
            <input v-model="confirmpassword" id="anse" placeholder="请确认密码" type="text">
            <input v-model="captcha_code" id="cd" placeholder="验证码" type="text">
            <img :src="code" alt="">
            <p id="p1" @click=" reset()">确认修改</p>
            <p id="p2" @click="getCode()">换一张</p>
        </div>
    </div>
</template>
<script>
export default {
  name: "password",
  data() {
    return {
      code: "", //存储验证码base64图片
      username: "",
      oldpassWord:"",
      newpassword:"",
      confirmpassword:"",
      captcha_code: "" //存储验证码输入框的值
    };
  },
  created() {
    //首先，获取验证码
    this.getCode();
  },
  methods: {
    getCode() {
      const api = "https://elm.cangdu.org/v1/captchas";
      this.$http({
        url: api,
        method: "post",
        //用于表示用户代理是否应该在跨域请求的情况下从其它域发送cookies --- 不使用缓存数据
        withCredentials: true
      }).then(res => {
        console.log("验证码数据");
        console.log(res);
        this.code = res.data.code;
      });
    },
    reset() {
      const api = "https://elm.cangdu.org/v2/changepassword";
      this.$http({
        url: api,
        method: "post",
        withCredentials: true,
        data: {
          username: this.username,
          oldpassWord: this.oldpassWord,
          newpassword:this.newpassword,
          confirmpassword:this.confirmpassword,
          captcha_code: this.captcha_code
        }
      }).then(res => {
        console.log("login数据");
        console.log(res);
        if (res.data.message) {
          alert(res.data.message); //登陆失败
        } 
        else {
           alert("密码修改成功");
          //登陆成功，跳转到首页,编程式路由跳转，传值query或者params传值
          
        }
      });
    },
    back(){
      this.$router.back();
    }
  }
};
</script>
<style scoped>
#top{
    width: 3.75rem;
    height: 0.5rem;
    background-color:rgb(49,144,232);
    position: relative;
}
#img1{
    position: absolute;
    top: 0.15rem;
    left: 0.1rem;
    width: 0.12rem;
}
#login{
    height: 0.5rem;
    line-height: 0.5rem;
    width: 3.75rem;
    text-align: center;
    color: white;
    font-size: 0.18rem;
}
#center{
    margin-top: 0.2rem;
    width: 3.75rem;
    height: 1.8rem;
    background-color: yellow;
    position: relative;
}
#ac{
    width: 3.75rem;
    height: 0.6rem;
    border-bottom: 1px solid rgb(235,235,235);
    font-size:0.16rem;
    text-indent: 0.15rem;
}
#se{
    width: 3.75rem;
    height: 0.6rem;
    border-bottom: 1px solid rgb(235,235,235);
    font-size:0.16rem;
    text-indent: 0.15rem;
}
#nse{
    width: 3.75rem;
    height: 0.6rem;
    border-bottom: 1px solid rgb(235,235,235);
    font-size:0.16rem;
    text-indent: 0.15rem;
}
#anse{
    width: 3.75rem;
    height: 0.6rem;
    border-bottom: 1px solid rgb(235,235,235);
    font-size:0.16rem;
    text-indent: 0.15rem;
}
#cd{
    width: 3.75rem;
    height: 0.6rem;
    font-size:0.16rem;
    text-indent: 0.15rem;
}
#p1{
    position: absolute;
    top: 3.3rem;
    width: 3.3rem;
    background-color:lightgreen;
    margin-left: 0.17rem;
    text-align: center;
    height: 0.35rem;
    font-size: 0.16rem;
    color: white;
    line-height: 0.35rem;
    border-radius: 5px;
}
img{
    position: absolute;
    top: 2.65rem;
    left: 2.2rem;
}
#p2{
     position: absolute;
    top: 2.65rem;
    left: 3.1rem;
    font-size: 0.16rem;
}
#p3{
    position: absolute;
    top: 1.9rem;
    display: inline-block;
    width: 3rem;
    font-size: 0.16rem;
    color: red;
}
#password{
    position: absolute;
    top: 4rem;
    right: 0.3rem;
}
</style>