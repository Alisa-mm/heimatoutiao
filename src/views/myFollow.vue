<template>
  <div class="focus">
      <!-- 用封装好的头部 -->
      <hmheader title='我的关注'>
          <span slot="left" class="iconfont iconjiantou2" @click="$router.back()"></span>
      </hmheader>
      <!-- 关注列表 -->
      <div class="list">
          <div class="box" v-for="(item,index) in followList" :key='item.id'>
              <!-- 左边的图片 -->
              <img :src="item.head_img" alt="">
              <!-- 中间的模块 上面是昵称 下面是时间 -->
              <div class="center">
                  <p>{{item.nickname}}</p>
                  <span>{{item.create_date}}</span>
              </div>
              <!-- 右边取消关注-->
              <span @click="unFocusUser(item.id,index)">取消关注</span>
          </div>
      </div>
  </div>
</template>

<script>
import hmheader from '@/components/hmheader.vue'

import {getUserFollow,unfollowUser} from '@/api/user.js'

export default {
    data () {
        return {
            followList:[]
        }
    },
  components: {
    hmheader
  },
  async mounted () {
     let res =await getUserFollow();
     console.log(res);
     this.followList=res.data.data.map(value=>{
         value.head_img = 'http://127.0.0.1:3000'+value.head_img
         return value
     })
  },
  methods:{
     async unFocusUser(id,index){
         let res = await unfollowUser(id)
         console.log(res);
         if(res.data.message=='取消关注成功'){
             this.$toast.success(res.data.message)
            //  根据索引来删除
             this.followList.splice(index,1)
         }
         
      }
  }
  
}
</script>

<style lang='less' scoped>
.list{
    .box{
        display: flex;
        padding: 25px 10px;
        justify-content: space-between;
        align-items: center;
        border-bottom: 1px solid #ccc;
        > img{
            display: block;
            width: 50/360*100vw;
            height: 50/360*100vw;
            border-radius: 50%;
            padding: 0 10px;
        }
        > .center{
            flex: 1;
            > p {
                line-height: 30px;
                font-size: 14px;
            }
            > span {
                font-size: 12px;
                color:#999;
            }
        }
        > span {
            height: 30px;
            background-color: #eee;
            font-size: 13px;
            border-radius: 30px;
            line-height: 30px;
            padding: 0px 15px;
        }
    }
}
</style>