<template>
  <div id="app">
    <Header></Header>
    <div class="main">
      <router-view name="slidebar"></router-view>
      <router-view name="main"></router-view>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header";

export default {
  name: 'App',
  components: {
    Header,
  },
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  min-width: 1210px;
  background: #e1e1e1;
}
.main{
  width: 80%;
  margin: 0 auto;
}
</style>
