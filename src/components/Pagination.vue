<template>
  <div class="Pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="jduge" class="pagebtn">...</button>
    <button
      v-for="btn in pagebtns"
      :class="[{activeBtn:btn === currentPage}, 'pagebtn']"
      @click="changeBtn(btn)">

      {{btn}}
    </button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>

<script>
import $ from 'jquery'

export default {
  name: "Pagination",
  data(){
    return{
      pagebtns: [1,2,3,4,5,'...'],
      currentPage: 1,
      jduge: false
    }
  },
  methods: {
    changeBtn(page){
      console.log(page)
      if(typeof page !== "number"){
        switch (page.target.innerText){
          case '上一页':
            $('button.activeBtn').prev().click()
            break
          case '下一页':
            $('button.activeBtn').next().click()
            break
          case '首页':
            this.pagebtns = [1,2,3,4,5,'...']
            this.changeBtn(1)
            break
          default:
            break
        }
        return
      }
      if(page > 4){
        this.jduge = true
      }else if(page < 3){
        this.jduge = false
      }

      if(page === this.pagebtns[4]){
        this.pagebtns.shift() //删除第一个按钮
        this.pagebtns.splice(4,0,this.pagebtns[3]+1) //在最后添加一个按钮
      }else if(page === this.pagebtns[0] && page !== 1){
        this.pagebtns.splice(4,1) // 删除倒数第二个按钮
        this.pagebtns.unshift(this.pagebtns[0]-1) // 在一个按钮前面仔加一个按钮
      }
      this.currentPage = page;
      this.$emit('childByValue', this.currentPage)

    }
  }
}
</script>

<style scoped>
.Pagination{
  margin-top: 10px;
}
.Pagination button{
  background-color: #fff;
  border: 1px solid #ddd;
  color: #778087;
  border-radius: 3px;
  outline: none;
  height: 21px;
  cursor: pointer;
  padding: 0 2px;
  width: 55px;
  height: 29px;
}
.pagebtn{
  position: relative;
  bottom: 1px;
  width: 40px;
  margin: 0 2px;
}
.Pagination button.activeBtn{
  background: #80bd01;
  color: #fff;
}
</style>
