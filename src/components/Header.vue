<template>
  <div class="Header">
    <router-link :to="{name:'root'}">
      <img src="../assets/logo.svg" alt="">
    </router-link>

    <!--    <input type="text">-->
    <ul class="top">
      <li>
        <router-link :to="{name:'root'}">
          <a href="#">首页</a>
        </router-link>
      </li>
      <li><a href="#">新手入门</a></li>
      <li><a href="#">API</a></li>
      <li><a href="#">关于</a></li>
      <li><a href="#">注册</a></li>
      <li><a href="#">登录</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Header"
}
</script>

<style scoped>
.Header {
  background: #444;
  width: 100%;
  height: 50px;
}

.Header img {
  max-width: 120px;
  margin-left: 50px;
  margin-top: 10px;
}
ul{
  list-style: none;
  float: right;
  margin: 4px;
}
li{
  display: inline-block;
  padding: 10px 15px;
}
a{
  text-decoration: none;
  color: #ccc;
  font-size: 13px;
  text-shadow: none;
}
</style>
