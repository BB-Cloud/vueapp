<template>
  <div>
    <XHeader  title="设置" :left-options="{showBack: true}"></XHeader>
    <div class="template">
      <section class="btn">
        <XButton type="default" @click.native="logout()">退出登录</XButton>
      </section>

      <tabs></tabs>
    </div>

  </div>

</template>



<script type="text/ecmascript-6">

  import { Grid, GridItem,XHeader,XButton } from 'vux'
  import tabs from "../../../components/common/tabs.vue"
  import setCookie from "@/common/setCookie"





  export default {
    name:"apply",
    methods:{
      logout(){
        console.log(this.$store);
//        let userCode=setCookie.getCookie("isAutoLogin");
//        setCookie.setCookie("isAutoLogin","0")
        this.$store.commit("isAutoLogin",0)
        this.$router.push({path:"/login"})
      }
    },
    data(){
      return{

      }
    },
    components: {
      Grid,
      GridItem,
      XHeader,
      tabs,
      XButton,
    }


  }
</script>


<style lang="less" type="text/less" scoped>
  .btn{
    padding:0.58rem;
  }

</style>
