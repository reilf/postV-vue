<!--当前用户关注用户-->
<template>
  <div class="follow">
    <ul>
      <li v-for="(item,index) in fans">
        <div class="user-msg">
          <div class="img-container">
            <img :src="item.avatarUrl" />
          </div>
          <div class="name">{{item.fanName}}</div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Fans",
  props:{
    fans:{
      type:Array,
      default()
      {
        return []
      }
    }
  }
}
</script>

<style scoped lang="less">
.img-container{
  width: 50px;
  height: 50px;
  border-radius: 50%;
  overflow: hidden;
  margin: 0 10px 0 0;
  background-color: #9b9b9b;
  img{
    width:50px;
  }
}
.follow{
  padding: 0 20px 0 0;
  ul{
    display: flex;
    flex-wrap: wrap;
    li{
      display: flex;
      justify-content:space-between;
      align-items: center;
      padding: 20px;
      width: 300px;
      .user-msg{
        display: flex;
        align-items: center;
      }
      &:nth-child(odd)
      {
        border-bottom: 1px solid rgba(58, 142, 230,.2);
        border-right:1px solid rgba(58, 142, 230,.2);
      }
      &:nth-child(even)
      {
        border-bottom: 1px solid rgba(58, 142, 230,.2);
      }
    }
  }
}
</style>