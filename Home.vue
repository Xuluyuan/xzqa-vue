<template>
  <div>
    <!-- 顶部导航开始 -->
    <div>
      <mt-header title="学前端，到学问">
        <div slot="right" class="shotcut" v-if="this.$store.state.isLogined==0">
          <router-link to="/register">注册</router-link>
          <router-link to="/login">登录</router-link>
        </div>
        <div slot="right" class="shotcut" v-else>
          <!-- <router-link to="/register">注册</router-link> -->
          <router-link to="/" @click="quit" >注销</router-link>
        </div>
      </mt-header>
    </div>
    <!-- 顶部导航结束 -->
    <!-- 顶部选项卡开始 -->
    <div>
      <mt-navbar v-model="active" >
        <mt-tab-item  v-for="(v,k) of category" :key="k" :id="v.id.toString()">{{v.category_name}}</mt-tab-item>
      </mt-navbar>
    </div>
    <!-- 顶部选项卡结束 -->
    <!-- 面板开始 -->
    <div infinite-scroll-distance="20" v-infinite-scroll="loadMore" infinite-scroll-disabled="busy" infinite-scroll-immediate-check="true">
      <mt-tab-container v-model="active">
        <mt-tab-container-item :id="active.toString()">
          <!-- 单一文章信息开始 -->
          <div class="article" v-for="(article,i) of articles" :key="i">
            <!-- 文章标题开始--->
            <!-- <router-link to="`/article`"> -->
            <router-link :to="`/article/${article.id}`">
              <div class="article-title">{{article.subject}}</div>
              <!-- 文章标题结束 -->
              <!-- 文章图文内容开始 -->
              <div class="article-wrapper">
                <div class="article-img" v-if="article.image!=null">
                  <img v-lazy="article.image">
                </div>
                <div class="article-desc">
                  {{article.description}}
                </div>
              </div>
              <!-- 文章图文内容结束 -->
            </router-link>
          </div>
          <!-- 单一文章信息结束 -->
        </mt-tab-container-item>
      </mt-tab-container>
      <div v-if="page>pageCount" class="my_bottom">我是底线</div>
    </div>
    <!-- 面板结束 -->
    <!-- 底部选项卡开始 -->
    <div style="margin-top:55px">
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
<style scoped>
  .shotcut a{
    text-decoration: none;
    color:#fff;
    margin-left: 5px;
  }
  .article{
    border-bottom: 1px solid #999;
    margin: 10px;
    padding-bottom: 10px;
  }
  .article a{
    text-decoration: none;
    color: #000;
  }
  .article-title{
    font-size: 16px;
    font-weight: bolder;
    margin-bottom: 10px;
    color: #1a1a1a;
  }
  .article-wrapper{
    display: flex;
    align-items: center;
  }
  .article-img{
    margin-right: 15px;
  }
  .article-img img{
    width: 112px;
    border-radius: 5px;
  }
  .article-desc{
    font-size: 15px;
    font-weight: 400;
    line-height: 21px;
    height: 63px;
    overflow: hidden;
  }
  .my_bottom{
    color:#ccc;
    text-align: center;
    padding: 10px;
  }
</style>
<script>
export default {
  data(){
    return{
      active:'1',
      selected:"index",
      category:[],
      articles:[],
      busy:false,
      page:1,//页码的初始值,
      pageCount:0//存储总页数
    }
  },
  watch:{
    active(){
      this.articles=[]
      this.loadData(this.active,1)
    },
    selected(){
      if(this.selected=="index"){
        this.$router.push("/")
      }else{
        this.$router.push("/me")
      }
    }
  },
  mounted(){
    this.axios.get('/category').then(res=>{
      this.category=res.data.result
    }),
    // this.axios.get('/lists?cid='+this.active)
    // this.axios.get(`'/lists?cid=${this.active}'`)
    this.loadData(this.active,1)
  },
  methods:{
    quit(){
      // localStorage.clear()
      localStorage.setItem('isLogined',"0")
      
    },
    loadData(cid,page){
      //显示加载提示框
      this.$indicator.open({
        text:'加载中...',
        spinnerType:'double-bounce'
      });
       this.axios.get('/lists',{
          params:{cid:cid,page:page}
        }).then(res=>{
          let data=res.data.result;
          this.pageCount=res.data._pageCount
          data.forEach(item=>{
          if(item.image!=null){
              item.image=require(`../assets/article/${item.image}`)
          }
          this.articles.push(item)
        })
        // this.articles=data 和 this.articles.push(item)任选
        //关闭加载提示框
        this.$indicator.close();
      })
    },
    loadMore(){
      this.busy=true
      this.page++;
      if(this.page<=this.pageCount){
          this.loadData(this.active,this.page)
          this.busy=false;
      }
    }
  }
}
</script>
