<template>

  <div>
    <XHeader  title="加班详情" :left-options="{showBack: true}">
    </XHeader>

    <div class="template">

      <!--<scroller :height="fHeight" lock-x scrollbar-y  ref="scrollerBottom"  bounce @on-scroll-bottom="onScrollBottom">-->
      <!--<div class="box2">-->
      <group v-for="(l,index) in listData" :key="index">
        <cell title="加班日期"  v-show="l.otfdate" :value="l.otfdate"></cell>
        <cell title="扣休息(分)"  v-show="l.ot_koutime" :value="l.ot_koutime"></cell>
        <cell title="上班时间"  v-show="l.otstimetext" :value="l.otstimetext"></cell>
        <cell title="下班时间"  v-show="l.otetimetext" :value="l.otetimetext"></cell>
        <cell title="加班原因"  v-show="l.otreason" :value="l.otreason"></cell>
        <cell title="提交状态"  v-show="l.issendtext" :value="l.issendtext"></cell>
        <cell title="提交时间"  v-show="l.senddate" :value="l.senddate"></cell>
        <cell title="审批状态"  v-show="l.apstatustext" :value="l.apstatustext"></cell>
        <!--<cell title="审批时间"  v-show="l.apdate" :value="l.apdate"></cell>-->
        <!--<cell title="审批意见"  v-show="l.apnode" :value="l.apnode"></cell>-->
        <!--<cell title="创 建 人" v-show="l.createby"  :value="l.createby"></cell>-->
        <!--<cell title="创建时间"  v-show="l.createtime" :value="l.createtime"></cell>-->
        <!--<cell title="修改人"   v-show="l.updateby"  :value="l.updateby"></cell>-->
        <!--<cell title="修改时间"  v-show="l.updatetime"  :value="l.updatetime"></cell>-->
        <div style=" background: rgb(142, 172, 201);color:white;text-align:center">以上第{{index+1}}项</div>
      </group>
      <load-more tip="正在加载" v-show="loadMoreDom"></load-more>
      <div v-show="loadMoreFinish" class="loadMoreFinish">加载完毕</div>


      <div class="btnWrap">
        <x-button type="default" text="申请请假" @click.native="btn"></x-button>
      </div>

    </div>

  </div>


</template>

<script>



  import {TransferDom, Actionsheet, Group, XSwitch, Cell,XHeader,CellBox ,Datetime,XInput,XTextarea,XButton
    ,Toast,GroupTitle,Scroller,LoadMore,Loading
  } from 'vux'
  import qs from 'qs'
  import {getScrollTop,getWindowHeight,getScrollHeight} from "@/common/util"

  export default {
    name: '',
    mounted(){
      this.getListData(this.pageIndex)
      if(getScrollHeight() == getWindowHeight()){
        this.getListData(this.pageIndex+=1)
      }
      if(this.loadDataSwitch){
        window.addEventListener("scroll",this.scroll)
      }

    },
    computed:{
      isLoading(){
      },
      fHeight(){
        let fHeight= (parseFloat(document.documentElement.style.fontSize)+42)*0.95
        fHeight = (-fHeight).toString()
        return fHeight
      },
    },
    methods:{
      scroll(){
        var g = getScrollTop() + getWindowHeight()
        if (g  === getScrollHeight()) {
          console.log("到底部");
          if(this.loadDataSwitch){
            this.getListData(this.pageIndex+=1)
          }
        }
      },

      btn(){
        this.$router.go(-1)
      },

      getListData(pageIndex){
        const empName=window.localStorage.getItem("global_empname")
        const empId=window.localStorage.getItem("global_empid")
        const company=window.localStorage.getItem("comp_code")

        let formData={company:company, globalEmpId: empId ,pageIndex: pageIndex, size: 1}

        this.$http.post("/MobileService/MyApply.asmx/GetOutTimeRecord",formData,{showLoad:false})
          .then(r=>{
          console.log(r)
        let data= eval("(" + r.data.d + ")");
        data=data.data
        console.log(data);
        if(data){
          for(let d in data){
            this.loadMoreDom=true
            this.listData.push(data[d])
          }
        }else{
          this.loadDataSwitch=false
          this.loadMoreDom=false
          this.loadMoreFinish=true
        }


      }).catch(err=>{
          this.loadMoreDom=false
        this.loadMoreFinish=true
      })
      },
      detailBtn(){

      },
      formSubmit(){

      }
    },
    components: {
      Actionsheet,
      XSwitch,
      Group,
      Cell,
      XHeader,
      CellBox,
      Datetime,
      XInput,
      XTextarea,
      XButton,Toast,GroupTitle,Scroller,LoadMore,Loading
    },
    data () {
      return {
        loadDataSwitch:true,
        pageIndex:1,
        loadMoreDom:false,
        loadMoreFinish:false,
        fontHeight:0,
        scrollList:20,
        listData:[],
        msg: 'Welcome to Your Vue.js App',
      }
    },
    destroyed(){
      window.removeEventListener("scroll",this.scroll)
    },
  }

</script>


<style lang="less" type="text/less" scoped>
  .btnWrap{
    position: fixed;
    width: 100%;
    bottom: 0;
  button.weui-btn{
    border-radius:0 !important;
  }
  }
  .weui-cell {
    padding: 8px 16px !important;
    font-size:0.24rem !important;
  }
  .box2{
    /*padding-top:1rem*/
  }

  .loadMoreFinish{
    text-align: center;
    padding: 0.18rem 0;
  }
</style>
