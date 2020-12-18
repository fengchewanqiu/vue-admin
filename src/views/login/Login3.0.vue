<template>
  <div class="login-bg">
    <div id="particles"></div>
    
    <el-main class="login-containr">
      <!-- form -->       
       <el-form :model="ruleForm" :rules="rules" class="login-elform"  ref="ruleForm" label-position="left" label-width="60px">
          <el-row class="bt"><el-col><div class="grid-content ">ZERO后台管理系统</div></el-col></el-row>
          <el-form-item prop="username">
            <span slot="label"   class="labels">账  号</span>
            <el-input type="text" v-model="ruleForm.username" prefix-icon="el-icon-user-solid" placeholder="登录账号"  autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item class="labels" prop="password">
            <span slot="label"   class="labels">密  码</span>
             <el-input  type="password" v-model="ruleForm.password" prefix-icon="el-icon-setting" placeholder="登录密码"  autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item  class="labels" prop="code">
            <span slot="label"   class="labels">验证码</span>
            <el-row>
              <el-col :span="14">
                <el-input v-model="ruleForm.code"  prefix-icon="el-icon-setting" placeholder="验证码"></el-input>
              </el-col>	
              <el-col :span="9" style="float: right;line-height: 0;">
                <!-- <img class="code pointer" :src="ruleForm.src" @click="refreshCaptcha()" /> -->
              </el-col>	
            </el-row>
          </el-form-item>  
          <el-form-item class="btn">
            <el-button type="primary" @click="submitForm('ruleForm')" size="small">立即登录</el-button>
            <el-button @click="resetForm('ruleForm')" size="small">重置/取消</el-button>
          </el-form-item>
      </el-form>
    </el-main>
  </div>
</template>

<script>
import particles from 'particles.js'
import particlesJson from '@/assets/utils/particles.json'
import {onMounted, reactive,ref } from "@vue/composition-api"

export default {
  name: "Login",
  components: {},
  setup(props, {refs}){ //setup(props,{refs,emit,slots,parent,attrs}){   
    //context 包含了很多[  context.attrs       context.slots     context.parent      context.root      context.emit      context.refs],所以也可以这样用  
   //放置自定义函数 data 生命周期等
   // 定义参数 数组/对象用rexxxxx let obj = reactive({对象体}),普通的用ref  let xxx = ref('变量')
    let ruleForm = reactive({
      username: "",
      password: "",
      code: "",
      })
    let rules = reactive({
     // username:[{required: true,message: "请输入名称",trigger: "blur"},{type: 'string',pattern: /^\/./,message: '请填写正确路径',trigger: 'blur'}], 不使用自定义函数 参数一空，参数二类别规则
      username:[ {validator: checkUser,trigger:'blur'}],
      password: [{ validator: checkPass, trigger: "blur" }],
      code: [{ validator: checkCode, trigger: "blur" }],
      })
    
    // 定义函数 直接用函数名  function(参数){逻辑};尽量不要使用箭头函数 避免出错
    function checkUser  (rule, value, callback){
      if(value===""){
         callback(new Error("请输入登录名称"));
      }else{
        callback();
      }
    }
    function checkPass(rule, value, callback){
      if (value === "") {
        callback(new Error("请输入登录密码"));
      } else {
        callback();
      }
    }
    function checkCode(rule, value, callback){
      if (!value) {
        setTimeout(() => {
        // 请求验证码丙自动刷新
        }, 1000);
        callback(new Error("验证码不能为空"));
      }else{
        callback();
      }      
    }
    let submitForm=(formName=>{
        refs[formName].validate((valid) => {
        if (valid) {
          console.log(111111111,ruleForm)
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    })
    let resetForm=(formName=>{
      refs[formName].resetFields();
    })
    // 生命周期
    onMounted(()=>{
      	particlesJS('particles',particlesJson);
    })
    //所有参数都要return出去
    return {
      rules,
      ruleForm,
      checkUser,
      checkPass,
      checkCode,
      submitForm,
      resetForm,
    }
  }
};
</script>

<style lang="scss" scoped>
.login-bg {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background: url(../../assets/imgs/login.jpg);
  background-size:cover;
  #particles{
  position: absolute;
  width: 100%;
  height: 100%;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: 50% 50%;
}
  .login-containr {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .login-elform {
    min-width:330px;
    max-width: 30vw;
    padding: 20px 10px;
    background: rgba(0, 0, 0, 0.5);
    border-radius: 5px;
  }
.code{width: 100%; height: 40px;border-radius: 5px;}
.labels{ color: #fff;}
.btn{display: flex;justify-content: space-around;align-items: center;width:100%;}
.bt{
  color:#ffffff;display:flex;width:100%;margin:10px auto 20px;padding:5px 10px;text-align: center;
}

  .login-title {
    width: 200px;
    height: 150px;
    background: #007aff;
    padding: 30px;
    border-radius: 5px 0 0 5px;
  }
  .login-title p {
    color: #ffffff;
  }
  .title {
    font-size: 20px;
    color: #fff;
  }
}
</style>
