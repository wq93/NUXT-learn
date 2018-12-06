<template>
  <div class="page">Page is search
    <ul>
      <li v-for="item in $store.state.city.list">{{item}}</li>
    </ul>
  </div>
</template>
<script>
  import axions from 'axios'

  export default {
    layout: 'search',
    data() {
      return {
        list: []
      }
    },
    async asyncData() { // 请求异步数据
      let {status, data: {list}} = await axions.get('http://localhost:3000/city/list')
      return { // 不能使用this.list 得return返回 效果一样
        list
      }
    }
  }
</script>
<style lang="css">
</style>
