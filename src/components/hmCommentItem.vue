<template>
  <div class="commentItem">
       <!-- 添加当前组件 -->
      <commentItem v-if="UpLevel.parent" :UpLevel="UpLevel.parent" @replyComment="SendComment"></commentItem>
      <div class="top">
          <div class="left">
              <span>{{UpLevel.user.nickname}}</span> &nbsp;&nbsp;&nbsp;
              <span>2分钟前</span>
          </div>
          <span @click="SendComment(UpLevel)">回复</span>
      </div>
      <div class="bottom">{{UpLevel.content}}</div>
  </div>
</template>

<script>
export default {
    // 组件递归  在组件内部 自己调用自己 要设置名字
    name:'commentItem',
   props:['UpLevel'],//父组件传过来的UpLevel=comment.parent =>comment是每一条评论
   methods:{  
       SendComment(comment){//comment是一个形参 
           console.log(comment);
         // 发出事件 给父组件 把comment传给父组件 父组件再传给子组件   
           this.$emit('replyComment',comment)
       }
   }
}
</script>

<style lang='less' scoped>
    .commentItem{
        border: 1px solid #ccc;
        padding: 5px;
        .top{
            font-size: 12px;
            color: #aaa;
            display: flex;
            justify-content: space-between;
        }
        .bottom{
            font-size: 13px;
            line-height: 40px;
        }
    }
</style>