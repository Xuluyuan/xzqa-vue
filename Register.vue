<template>
  <!-- 顶部导航开始 -->
  <div>
    <mt-header title="学前端，到学问">
      <router-link to="/" slot="left">
        <mt-button icon="back">
          <img src="../assets/images/homepage.png" slot="icon">
        </mt-button>
      </router-link>
      <router-link to="/login" slot="right">
        <mt-button >登录</mt-button>
      </router-link>
    </mt-header>
    <!-- 顶部导航结束 -->
  <!-- 注册页面开始 -->
  <div>
    <mt-field type="text" label="用户名" placeholder="请输入用户名" :attr="{maxlength:10}" v-model="uname" @blur.native.capture="checkUname" :state="unameState"></mt-field>
    <mt-field type="password" label="密码" placeholder="请输入密码" :attr="{maxlength:10}" autocomplete='off' v-model="upwd" @blur.native.capture="checkUpwd" :state="upwdState"></mt-field>
    <mt-field id="again" type="password" label="确认密码" placeholder="请再次输入密码" :attr="{maxlength:10}" autocomplete='off' v-model="againUpwd" @blur.native.capture="checkagainUpwd" :state="againUpwdState"></mt-field>
    <div class="reg" >
      <mt-button type="primary" size="large" class="reg"  @click="handle">免费注册</mt-button>
    </div>
  </div>
  <!-- 注册页面结束 -->
  </div>
  
</template>
<style  scoped>
  .reg{
    text-align: center;
  }
</style>
<script>
export default {
  data(){
    return{
      uname:"",
      upwd:"",
      againUpwd:"", 
      unameState:"",
      upwdState:"",
      againUpwdState:""
    }
  },
  methods:{
    checkUname(){
      let unameReg= /^[\w\._]{6,10}$/
     if(unameReg.test(this.uname)){
       this.unameState="success"
       return true
     }else if(this.uname==""){
       this.$toast({
         message:'用户名是必填项',
         position:'top'
       })
       this.unameState="error"
       return false
     }else{
        this.$toast({
        message:"格式不正确(6~10位数字/字母/下划线/.的组合)",
        position:'top',
        duration:'2000',
      })
        this.unameState="error"
        return false
     }
    },
    checkUpwd(){
      let upwdReg=/^[\w]{8,15}/
      if(upwdReg.test(this.upwd)){
        this.upwdState="success"
        return true
      }else if(this.upwd==""){
        this.$toast({
          message:'密码不能为空',
          position:'top'
        })
        this.upwdState="error"
        return false
      }else{
        this.$toast({
          message:'格式不正确(8~15位数字/字母组合)',
          position:'top',
          duration:'2000'
        })
        this.upwdState="error"
        return false
      }
    },
    checkagainUpwd(){
      if(this.upwd!==this.againUpwd){
        this.$toast({
          message:'两次密码不一致',
          position:'top',
          duration:'2000'
        })
        this.againUpwdState="error"
        return false
      }else if(this.againUpwd==""){
        this.$toast({
          message:'二次密码不能为空',
          position:'top'
        })
        this.againUpwdState="error"
        return false
      }else{
        this.againUpwdState="success"
        return true
      }
    },
    handle(){
      if(this.checkUname()&&this.checkUpwd()&this.checkagainUpwd()){
        this.axios.post('/reg',`username=${this.uname}&password=${this.upwd}`).then(res=>{
          if(res.data.code==1){
            this.$router.push("/login")
          }else{
            // alert(res.data.message)
            this.$messagebox("注册提示","用户名已占用")
          }
        })
      }
    }
  }
}
</script>