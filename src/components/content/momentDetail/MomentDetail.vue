<template>
  <div class="moment-detail">
    <div class="left-content">
      <moment :momentDetail="this.$store.state.momentDetails"
              v-if="this.$store.state.momentDetails.momentId!==undefined">
        <div slot="momentContent">
          <div class="content">
            <div class="state">
              <span v-html="this.$store.state.momentDetails.content">
                {{this.$store.state.momentDetails.content}}
              </span>
            </div>
          </div>
        </div>
      </moment>
    </div>
    <div class="right-content">
      <avatar :user-id="userId"/>
      <spcolumn :user-id="userId"
                :momentId="this.$store.state.momentDetails.momentId"
                 @spcolumn-change="spcolumnChange"/>
    </div>
  </div>
</template>

<script>
import Moment from "@/components/content/moment/Moment";
import Avatar from "@/components/content/avatar/Avatar";
import Spcolumn from "@/components/content/momentDetail/childCpn/spColumn/Spcolumn";
export default {
  name: "MomentDetail",
  components: {Avatar, Moment,Spcolumn},
  data()
  {
    return {
      userId:'',
      momentId:'',
      momentDetail:{}
    }
  },
  created() {
    this.userId=this.$route.query.userId;
    this.momentId=this.$route.query.momentId;
    //获取动态详情
    this.$store.dispatch({
      type:'getMomentDetail',
      momentId:this.momentId
    })
  },
  methods:{
    spcolumnChange(momentId){
      this.momentId=momentId;
      this.$store.dispatch({
        type:'getMomentDetail',
        momentId:this.momentId
      })
    }
  }
}
</script>

<style scoped lang="less">
   .moment-detail{
     width: 1180px;
     display: flex;
     overflow: hidden;
   }
  .content{
    .state{
      color: #595959;
      line-height: 30px;
      /deep/ img{
        width: 350px;
        outline: 1px solid rgba(0,0,0,.1);
        display: block;
      }
    }
  }
  .left-content{
    width:70%;
    border-right: 1px solid rgba(58, 142, 230,.2);
    padding: 0 20px 0 0;
  }
  .right-content{
    flex: 1;
    padding: 0 10px;
  }
</style>