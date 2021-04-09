<template>
  <div class="UserInfo">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="">
    </div>

    <div class="user_info" v-else>
      <router-link :to="{
        name: 'root',
      }">
        <div class="header">主页 /</div>
      </router-link>
      <div class="user_content">
        <span class="user">
          <img :src="post.avatar_url" alt="">
          <span class="user_name">{{ post.loginname }}</span>
      </span>
        <div class="score">{{ post.score }} 积分</div>
        <p class="create_time">
          注册时间 {{ post.create_at | formatDate }}
        </p>
      </div>
    </div>

    <div class="recent_topics">
      <div class="header">最近参与的话题</div>
      <div class="topics_content">
        <ul>
          <li v-for="item in post.recent_topics">
            <router-link :to="{
              name: 'user_info',
              params: {
                name: item.author.loginname
              }
            }">
              <img :src="item.author.avatar_url" alt="">
            </router-link>
            <router-link :to="{
              name: 'post_content',
              params: {
                id: item.id,
              }
            }">
              <span class="post_title">{{ item.title }}</span>
            </router-link>
            <span class="last_reply">{{ post.create_at | formatDate }}</span>
          </li>
        </ul>
      </div>
<!--      <div class="more_topics">
        <router-link :to="{

        }">
          查看更多»
        </router-link>
      </div>-->
    </div>

    <div class="recent_replies">
      <div class="header">最近参与的回复</div>
      <div class="replies_content">
        <ul>
          <li v-for="item in post.recent_replies">
            <router-link :to="{
              name: 'user_info',
              params: {
                name: item.author.loginname
              }
            }">
              <img :src="item.author.avatar_url" alt="">
            </router-link>

            <router-link :to="{
              name: 'post_content',
              params: {
                id: item.id,
              }
            }">
              <span class="post_title">{{ item.title }}</span>
            </router-link>
            <span class="last_reply">{{ post.create_at | formatDate }}</span>
          </li>
        </ul>
      </div>
<!--      <div class="more_replies">查看更多»</div>-->
    </div>
  </div>
</template>

<script>
export default {
  name: "UserInfo",
  data() {
    return {
      isLoading: false,
      post: {},
    }
  },
  methods: {
    getData() {
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then((res) => {
          this.isLoading = false
          this.post = res.data.data
          console.log('SlideBar'+res)
        })
        .catch((err) => {
          console.log(err)
        })
    }
  },
  beforeMount() {
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

<style scoped>
.UserInfo{
  font-size: 14px;
  border-radius: 5px;
  margin-right: 340px;
  margin-top: 15px;
}
ul {
  list-style: none;
}
a{
  text-decoration: none;
}
.user img{
  vertical-align: middle;
}

.user .user_name{
  padding-left: 5px;
  font-size: 11px;
  font-weight: 700;
  color: #778087;
}
.user_content .score{
  padding: 10px 0;
}
.user_content .create_time{
  color: #ababab;
}
.user_info,
.recent_topics,
.recent_replies {
  margin-top: 15px;
}

.user_info img,
.recent_topics img {
  width: 40px;
  height: 40px;
  border-radius: 3px;
}

.user_info > a > .header,
.recent_topics .header,
.recent_replies > .header,
.more_topics,
.more_replies{
  padding: 10px;
  background: #f6f6f6;
  border-radius: 3px 3px 0 0;
  color: #444;
}

.user_info a .header {
  color: #80bd01;
  cursor: pointer;
}

.user_content {
  padding: 10px;
  background: #fff;
  border-top: 1px solid #e5e5e5;
}

.user_content,
.topics_content,
.replies_content{
  background: #fff;
}

.recent_topics img,
.recent_replies img {
  width: 30px;
  height: 30px;
  border-radius: 3px;
  vertical-align: middle;
}

.recent_topics li,
.recent_replies li {
  border-top: 1px solid #f0f0f0;
  padding: 10px;
  white-space: nowrap;
}

.post_title{
  padding-left: 20px;
}

.recent_topics li a,
.recent_replies li a {
  text-decoration: none;
  max-width: 70%;
  white-space: nowrap;
  display: inline-block;
  vertical-align: middle;
  font-size: 16px;
  color: #08c;
}

.more_topics,
.more_replies{
  color: #666;
}

.last_reply {
  float: right;
  text-align: right;
  margin-top: 10px;
  font-size: 11px;
  color: #777;
}
</style>
