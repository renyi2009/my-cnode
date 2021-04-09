<template>
  <div class="PostList">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="">
    </div>
    <div class="posts" v-else>
      <ul>
        <li class="post topbar">
          <span @click="setTab()" :class="{active: !tab}" >全部</span>
          <span @click="setTab('good')" :class="{active: tab === 'good'}" >精华</span>
          <span @click="setTab('share')" :class="{active: tab === 'share'}" >分享</span>
          <span @click="setTab('ask')" :class="{active: tab === 'ask'}">问答</span>
          <span @click="setTab('job')" :class="{active: tab === 'job'}">招聘</span>
        </li>
        <li class="post" v-for="post in posts">
          <router-link :to="{
              name: 'user_info',
              params: {
                name: post.author.loginname
              }
            }">
            <img :src="post.author.avatar_url" alt="">
          </router-link>
          <span class="count">
            <span class="reply_count">{{ post.reply_count }}</span>/<span
            class="visit_count">{{ post.visit_count }}</span>
          </span>
          <!--          标签-->
          <span :class="[{put_good:(post.good === true), put_top:(post.top === true),
          'topiclist-tab':(post.good !== true && post.top !== true)}]">
            <span>{{ post | tabFormatter }}</span>
          </span>
          <router-link class="post_title" :to="{
              name: 'post_content',
              params: {
                id: post.id,
                name: post.author.loginname,
              }
            }">
            {{ post.title }}
          </router-link>
          <span class="last_reply">
            {{ post.create_at | formatDate }}
          </span>
        </li>
      </ul>
    </div>
    <div>
      <pagination v-on:childByValue="childByValue"></pagination>
    </div>
  </div>
</template>

<script>
import pagination from './Pagination'
import $ from "jquery";

export default {
  name: "PostList",
  data() {
    return {
      isLoading: false,
      posts: [],
      currentPage: 1,
      tab: null
    }
  },
  components: {
    pagination
  },
  methods: {
    setTab(tab) {
      this.tab = tab
      this.getData()
    },
    getData() {
      this.posts = []
      let params = {
        page: this.currentPage,
        limit: 20
      }
      if(this.tab) {
        params.tab = this.tab
      }
      this.$http.get('https://cnodejs.org/api/v1/topics', {
        params: params
      })
        .then((res) => {
          if (res.data.success === true) {
            this.posts = res.data.data
            // console.log(res)
          }
        })
        .catch((err) => {
          console.log(err)
        })
    },
    childByValue(val) {
      this.currentPage = val
      this.getData()
    }
  },
  beforeMount() {
    this.isLoading = false
    this.getData()
  }
}
</script>

<style scoped>
.PostList {
  border-radius: 5px;
  /*margin-right: 340px;*/
  margin-top: 15px;
}

ul {
  list-style: none;
  background: #fff;
  width: 100%;
  text-overflow: ellipsis;
  border-radius: 5px;
}

a {
  text-decoration: none;
}

.posts {
  min-width: 624px;
}

.topbar .active {
  background-color: #80bd01;
  color: #fff;
  padding: 3px 4px;
  border-radius: 3px;
}

.topbar {
  padding: 10px;
  background-color: #f6f6f6;
  border-radius: 3px 3px 0 0;
}

.topbar span {
  text-decoration: none;
  cursor: pointer;
  color: #80bd01;
  margin: 0 10px;
}


.count {
  display: inline-block;
  min-width: 75px;
  font-size: 12px;
  text-align: center;
}

.reply_count {
  color: #9e78c0;
  font-size: 14px;
}

.visit_count {
  color: #b4b4b4;
  font-size: 10px;
}

.posts img {
  width: 30px;
  height: 30px;
  border-radius: 3px;
  vertical-align: middle;
}

.put_good, .put_top {
  font-size: 12px;
  color: #fff;
  background: #80bd01;
  padding: 2px 4px;
  border-radius: 3px;
}

.topiclist-tab {
  font-size: 12px;
  color: #999;
  background: #e5e5e5;
  padding: 2px 4px;
  border-radius: 3px;
}

li.post {
  width: 100%;
  border-top: 1px solid #f0f0f0;
  font-size: 14px;
  padding: 10px;
  display: inline-block;
  white-space: nowrap;
}

li.post a.post_title {
  max-width: 70%;
  vertical-align: middle;
  font-size: 16px;
  color: #333;
  display: inline-block;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}

.post_title:hover {
  text-decoration: underline;
}

.last_reply {
  float: right;
  text-align: right;
  padding-top: 8px;
  min-width: 50px;
  color: #778087;
  font-size: 11px;
}

.loading {
  text-align: center;
  padding-top: 300px;
}
</style>
