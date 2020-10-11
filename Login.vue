<template>
<div>
  <mt-header title="用户登录">
    <router-link slot="left" to="/">
      <mt-button>
        <img src="../assets/images/homepage.png" slot="icon">
      </mt-button>
    </router-link>
    <router-link to='/register' slot="right">
      <mt-button>注册</mt-button>
    </router-link>
  </mt-header>
  <div>
    <mt-field type="text" placeholder="请输入用户名" label="用户名" v-model="uname" @blur.native.capture="checkUname" :state="unameState"></mt-field>
    <mt-field type="password" placeholder="请输入密码" label="密码" v-model="upwd" @blur.native.capture="checkUpwd" :state="upwdState"></mt-field>
    <mt-button type="primary" size="large" @click="holder">登录</mt-button>
  </div>
</div>
</template>
<script>
export default {
  data(){
    return{
      uname:"",
      upwd:"",
      upwdState:"",
      unameState:""
    }
  },
  methods:{
    checkUname(){
      let unameReg=/^[\w\._]{6,10}$/
      if(unameReg.test(this.uname)){
        this.unameState="success"
        return true
      }else{
        this.$toast({
          message:'用户名不正确',
          position:'top',
          duration:'2000'
        })
        this.unameState="error"
        return false
      }
    },
    checkUpwd(){
      let upwdReg=/^[\w]{8,15}$/
      if(upwdReg.test(this.upwd)){
        this.upwdState="success"
        return true
      }else{
        this.$toast({
          message:'密码不正确',
          position:'top',
          duration:'2000'
        })
        this.upwdState="error"
        return false
      }
    },
    holder(){
      if(this.checkUname()&&this.checkUpwd()){
        let obj={
          username:this.uname,
          password:this.upwd,
          // love:['aaa','bbb','ccc']
        }
        // console.log(this.qs.stringify(obj))//username=dsddddd&password=111111111
        // console.log(decodeURI(this.qs.stringify(obj)))//username=dsddddd&password=111111111&love[0]=aaa&love[1]=bbb&love[2]=ccc
        this.axios.post('/login',this.qs.stringify(obj)).then(res=>{
          if(res.data.code==1){
            //提交mutations
            this.$store.commit("logined")
            //必须要将数据存储到webStorge中，因为用户刷新后记录依旧要保持
            localStorage.setItem('isLogined',"1")
            this.$router.push("/")
          }else{
            console.log(res.data.message)
            this.$messagebox("登录信息","用户名或密码错误")
          }
        })
      }
    }
  }

}
</script>