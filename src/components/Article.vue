<template>
  <div class="Article">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="">
    </div>
    <div v-else>
      <div class="post_header">
        <h2 class="post_title">{{post.title}}</h2>
        <ul class="changes">
          <li>• 发布于：{{post.create_at | formatDate}}</li>
          <li>• 作者：
            {{post.author.loginname}}
          </li>
          <li>• {{post.visit_count}}次浏览</li>
          <li>• 来自 {{post | tabFormatter}}</li>
        </ul>
        <div class="post_content" v-html="post.content"></div>
      </div>
      <div class="reply">
        <div class="reply_title">{{post.replies.length}} 回复</div>
        <ul>
          <li v-for="(reply, index) in post.replies">
            <router-link :to="{
              name: 'user_info',
              params: {
                name: reply.author.loginname
              },
            }">
              <img :src="reply.author.avatar_url" alt="">
              <span>{{reply.author.loginname}}</span>
            </router-link>
            <span class="reply_time">
              <span>{{index+1}}楼•</span>
              <span>{{post.create_at | formatDate}}</span>
            </span>
            <p class="reply_content" v-html="reply.content"></p>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Article",
  data(){
    return {
      isLoading: false,
      post: {}
    }
  },
  methods: {
    getData() {
      this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
        .then((res)=>{
          if(res.data.success === true){
            this.isLoading = false
            this.post = res.data.data
            console.log(res)
          }
        })
        .catch((err)=>{
          console.log(err)
        })
    }
  },
  beforeMount() {
    console.log('---')
    this.isLoading = true
    this.getData()
  },
  watch: {
    '$route'(to, from){
      this.getData()
    }
  }
}
</script>

<style>
@import url('../assets/markdown-github.css');
ul{
  list-style: none;
}

.Article{
  border-radius: 5px;
  margin-right: 340px;
  margin-top: 15px;
}
.post_header,
.post_content,
.reply{
  background: #fff;
  margin-bottom: 10px;
}

.post_header{
  padding: 10px;
  border-radius: 3px 3px 0 0;
}
.post_header li{
  display: inline-block;
}
.changes{
  font-size: 12px;
  padding-top: 10px;
  color: #838383;
}

.post_content{
  border-top: 1px solid #e5e5e5;
  border-radius: 0 0 3px 3px;
  padding: 20px;
}
.reply{
  border-radius: 3px;
  margin-top: 15px;
  font-size: 14px;
  color: #444;
 }
.reply img{
  vertical-align: middle;
  width: 30px;
  height: 30px;
  border-radius: 3px;
}
.reply a{
  font-size: 12px;
  font-weight: 700;
  text-decoration: none;
  color: #666
}
.reply_title{
  padding: 10px;
  background-color: #f6f6f6;
  border-radius: 3px 3px 0 0;
}
.reply li{
  border-top: 1px solid #f0f0f0;
  font-size: 14px;
  padding: 10px;
}
.reply_time{
  color: #08c;
  font-size: 11px;
  margin-top: 5px;
}
.reply_content{
  font-size: 15px;
  overflow: auto;
  padding-left: 50px;
}
.markdown-text img {
  width: 92% !important;
}
</style>
