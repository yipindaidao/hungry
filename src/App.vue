<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="nav">
      <div class="item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
  import header from './components/header/header'

export default {
  components: {
    'v-header': header
  },
    created() {
        this.$http.get('/api/seller').then(res => {
            res = res.body
            this.seller = res.data
        })
    },
  data() {
    return {
        seller: {}
    }
  }
}
</script>

<style>
  @import "./common/style/font.css";
  .nav {
    display: flex;
    height: 40px;
    line-height: 40px;
    text-align: center;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
  }

  .nav .item {
    flex: 1;
    font-size: 14px;

  }

  .nav .item a {
    color: rgb(77, 85, 93);
  }

  .nav .item a.active {
    color: rgb(240, 20, 20);
  }

</style>
