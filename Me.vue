<template>
  <div>
    <!-- 顶部导航开始 -->
    <div>
      <mt-header title="学前端，到学问">
        <router-link to="/" slot="left">
          <mt-button icon="back">
            <img src="../assets/images/homepage.png" slot="icon">
          </mt-button>
        </router-link>
      </mt-header>
    </div>
    <!-- 顶部导航结束 -->
    <!-- 底部选项卡开始 -->
    <div>
      <mt-tabbar v-model="selected" fixed>
        <mt-tab-item id="index">
          首页
          <img v-if="selected=='index'" src="../assets/images/index_enabled.png" alt="" slot="icon">
          <img v-else src="../assets/images/index_disabled.png" alt="" slot="icon">
        </mt-tab-item>
        <mt-tab-item id="me">
          我的
          <img v-if="selected=='me'" src="../assets/images/me_enabled.png" alt="" slot="icon">
          <img v-else src="../assets/images/me_disabled.png" alt="" slot="icon">

        </mt-tab-item>
      </mt-tabbar>
    </div>
    <!-- 底部选项卡结束 -->
  </div>
</template>
<script>
export default {
  data(){
    return{
      selected:"me"
    } 
  },
  watch:{
    selected(){
      if(this.selected=="me"){
        this.$router.push("/me")
      }else{
        this.$router.push("/")
      }
    }
  }
}
</script>