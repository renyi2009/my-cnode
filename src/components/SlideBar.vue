<template>
  <div class="SlideBar">
    <div class="author_info">
      <div class="header">作者</div>
      <div class="content">
        <router-link :to="{
          name: 'user_info',
          params: {
            name: post.loginname
          }
        }">
          <span class="user">
            <img :src="post.avatar_url" alt="">
            <span class="user_name">{{ post.loginname }}</span>
          </span>
        </router-link>

        <div class="score">积分: {{ post.score }}</div>
      </div>
    </div>


    <div class="author_topics">
      <div class="header">作者最近参与的话题</div>
      <div class="content">
        <ul>
          <li v-for="item in topiclimitby5">
            <router-link :to="{
              name: 'post_content',
              params: {
                id: item.id,
                name: item.author.loginname
              }
            }">
              {{item.title}}
            </router-link>
          </li>
        </ul>
      </div>
    </div>



    <div class="author_replies">
      <div class="header">作者最近回复的话题</div>
      <div class="content">
        <ul>
          <li v-for="item in replylimitby5">
            <router-link :to="{
              name: 'post_content',
              params: {
                id: item.id,
                name: item.author.loginname
              }
            }">
              {{item.title}}
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SlideBar",
  data() {
    return {
      post: {},
    }
  },
  computed: {
    topiclimitby5(){
      if(this.post.recent_topics){
        return this.post.recent_topics.slice(0,5)
      }
    },
    replylimitby5(){
      if(this.post.recent_replies){
        return this.post.recent_replies.slice(0,5)
      }
    },
  },
  methods: {
    getData() {
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          this.post = res.data.data
          console.log(res)
        })
        .catch(err => {
          console.log(err)
        })
    }
  },
  beforeMount() {
    this.getData()
  },
  watch: {
    '$route'(to, from){
      this.getData()
    }
  }
}
</script>

<style scoped>
.SlideBar{
  width: 328px;
  float: right;
  margin-top: 0;
  font-size: 14px;
  margin-left: 10px;
}
a{
  text-decoration: none;
  /*font-size: 12px;*/
  color: #778087;
}
ul{
  list-style: none;
}
li{
  margin-bottom: 10px;
}
.author_info img{
  width: 48px;
  height: 48px;
  border-radius: 3px;
  vertical-align: middle;
  margin-bottom: 10px;
  margin-right: 10px;
}
/*.author_info .user_name{
  color: #778087;
}*/

.author_info,
.author_topics,
.author_replies {
  margin-bottom: 10px;
  border-radius: 3px;
  /*border: 1px solid blue;*/
}

.author_info .header,
.author_topics .header,
.author_replies .header {
  background: #f6f6f6;
  border-radius: 3px 3px 0 0;
  padding: 10px;
  color: #444;
}
.author_info .content,
.author_topics .content,
.author_replies .content{
  background: #fff;
  padding: 10px;
}

.author_topics li a,
.author_replies li a{
  color: #778087;
}

</style>
