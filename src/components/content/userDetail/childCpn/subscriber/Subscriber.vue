<template>
  <div class="subscriber">
    <div class="content">
      <tab-control :list="['动态','专题','频道']">
        <ul class="moment" slot="动态">
          <li v-for="(item,index) in momentDetails" :key="item.momentId">
            <div class="moment-left">
              <div class="moment-img-container" @click="momentRouter(item)">
                <img :src="item.picUrl[0].picUrl" alt="" />
              </div>
            </div>
            <div class="moment-right">
              <div class="title text-nowrap">{{item.title}}</div>
              <div class="msg">
                <div class="user-msg" v-if="item.user">
                  <div class="avatar">
                    <img :src="item.user.avatarUrl" alt=""/>
                  </div>
                  <div class="user-name">{{item.user.userName}}</div>
                </div>
                <div class="time">{{item.updateTime.substring(0,10)}}</div>
              </div>
            </div>
          </li>
        </ul>
        <ul class="topic" slot="专题">
          <li v-for="(item,index) in topicDetails" :key="item.topic_content_id">
            <div class="topic-left">
              <div class="title" @click="topicRouter(item)">{{item.title}}</div>
              <div class="msg">
                <div class="user-msg" v-if="item.user">
                  <div class="avatar">
                    <img :src="item.user.avatarUrl" alt=""/>
                  </div>
                  <div class="user-name">{{item.user.userName}}</div>
                </div>
              </div>
            </div>
            <div class="topic-right">
              <div class="topic-container-img" v-if="item.picUrl">
                <img :src="item.picUrl[0].picUrl" alt=""/>
              </div>
            </div>
          </li>
        </ul>
        <ul class="channel" slot="频道">
          <li v-for="(item,index) in channelDetail" :key="item.id">
            <div class="img-container" @click="channelRouter(item,index)">
              <img :src="item.picUrl" alt=""/>
            </div>
            <div class="msg">
              <div class="title text-nowrap" @click="channelRouter(item,index)">{{item.title}}</div>
              <div class="user-and-time">
                <UserMsg :user-name="item.user.userName" :avatar-url="item.user.avatarUrl"/>
                <div class="time">{{formatTime(item.createTime,"yyyy-MM-dd")}}</div>
              </div>
            </div>
          </li>
        </ul>
      </tab-control>
    </div>
  </div>
</template>

<script>
import MomentDetail from "@/components/content/momentDetail/MomentDetail";
import Moment from "@/components/content/moment/Moment";
import {getUserSub} from "@/network/user";
import TabControl from "@/components/common/tabControl/TabControl";
import UserMsg from "@/components/common/userMsg/UserMsg";
import {formatDate} from "@/utils/formatDate";

export default {
name: "Subscriber",
  components: {UserMsg, TabControl, Moment, MomentDetail},
  props:{
    userId:{
      type:String,
      default()
      {
        return ''
      }
    },
  },
  data()
  {
    return {
      momentDetails:[],
      topicDetails:[],
      channelDetail:[],
      user:{}
    }
  },
  created() {
  //console.log(this.userId)
    getUserSub(this.userId).then(data=>{
      //console.log(data);
      this.momentDetails=data.moments
      this.topicDetails=data.topicContent;
      this.channelDetail=data.channel;
      this.user=data.user;
    })
  },
  methods:{
    formatTime(time,ft){
      return formatDate(time,ft);
    },
    momentRouter(item)
    {
      console.log(item);
      if(item.type===0)
      {
        this.$router.push({
          path:'/momentDetail',
          query:{
            momentId:item.momentId,
            userId:item.user.userId
          }
        })
      }
      else if(item.type===1)
      {
        this.$router.push({
          path:'/videoDetail',
          query:{
            vid:item.vid
          }
        })
      }
    },
    topicRouter(item)
    {
      this.$router.push({
        path:'/topicContentDetail',
        query:{
          topic_content_id:item.topic_content_id
        }
      })
    },
    channelRouter(item,index){
      this.$router.push({
        path:"/channelDetail",
        query:{
          cId:item.id
        }
      })
    }
  }
}
</script>

<style scoped lang="less">
  .center()
  {
    position: absolute;
    left:50%;
    top:50%;
    transform: translate(-50%,-50%);
  }
  .moment{
    li{
      display: flex;
      padding: 10px 0;
      .moment-left{
        .moment-img-container{
          width:168px;
          height: 104px;
          overflow: hidden;
          background-color: #333;
          position: relative;
          border-radius: 4px;
          img{
            .center();
            width: 200px;
          }
        }
      }
      .moment-right{
        margin: 0 0 0 20px;
        background-color: #fff;
        width: 60%;
        .title{
          width:100%;
          font-size: 16px;
          font-weight: bold;
          margin: 0 0 20px 0;
        }
        .msg{
          display: flex;
          width: 100%;
          align-items: center;
          justify-content:space-between;
          .user-msg{
            display: flex;
            align-items: center;
            .avatar{
              width: 20px;
              height: 20px;
              overflow: hidden;
              border-radius: 50%;
              position: relative;
              img{
                .center();
                height: 20px;
              }
            }
            .user-name{
              font-size: 12px;
              color: #3a8ee6;
            }
          }
          .time{
            color: #9ca3ab;
            font-size: 12px;
          }
        }
      }
    }
  }
  .topic{
    li{
      display: flex;
      justify-content: space-between;
      padding: 10px 30px 10px 0;
      border-bottom: 1px solid #d8e8fa;
    }
    .topic-left{
      .title{
        font-size: 20px;
        margin: 0 0 30px 0;
        cursor:pointer;
      }
      .msg{
        .user-msg{
          display: flex;
          align-items: center;
          .avatar{
            width: 20px;
            height: 20px;
            overflow: hidden;
            border-radius: 50%;
            position: relative;
            img{
              .center();
              height: 20px;
            }
          }
          .user-name{
            color: #3a8ee6;
            font-size: 12px;
          }
        }
      }
    }
      .topic-right{
        .topic-container-img{
          width: 120px;
          height: 80px;
          overflow: hidden;
          border-radius: 8px;
          background-color: #333;
          img{
            width: 120px;
          }
        }
    }
  }
  .channel{
    &>li{
      display: flex;
      padding: 10px 0;
      .title{
        font-size: 16px;
        font-weight: bold;
        width: 100%;
      }
      .user-and-time{
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin: 20px 0 0 0;
      }
      .msg{
        margin: 0 0 0 20px;
        width: 60%;
        .time{
          font-size: 12px;
          color:#656565;
        }
      }
    }
    .img-container{
      width: 168px;
      height: 104px;
      overflow: hidden;
      border-radius: 4px;
      img{
        width: 100%;
      }
    }
  }
</style>