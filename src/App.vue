<template>
  <div id="app">
    <v-header :seller='seller'></v-header>
    <div class="nav border-1px">
      <div class="nav-item">
        <router-link :to='{path:"/goods"}'>商品</router-link>
      </div>
      <div class="nav-item">
        <router-link :to='{path:"/ratings"}'>评论</router-link>
      </div>
      <div class="nav-item">
        <router-link :to='{path:"/seller"}'>商家</router-link>
      </div>
    </div>
    <div class="content">
      <keep-alive>
      <router-view :seller="seller"></router-view>
      </keep-alive>
    </div>
  </div>
</template>

<script type="text/ECMAScript-6">
  import header from './components/header/header';
  import {urlParse} from './common/js/urlParse';
  const ERR_OK = 0;
  export default {
    data() {
      return {
        seller: {
          id: (() => {
            let urlParams = urlParse();
            return urlParams.id;
          })()
        }
      };
    },
    created() {
      this.$http.get('/api/seller?id=' + this.seller.id).then((res) => {
       res = res.body;
       if (res.errno === ERR_OK) {
          this.seller = Object.assign({}, this.seller, res.data);
       }
      });
    },
    // 在gitHub上的版本
    // his.$http.get('static/data.json).then((res) => {
    //       this.seller = res.data.seller;
    //   });
    // },
    components: {
      'v-header': header
    }
  };
</script>

<style type="text/stylus" lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixins.styl";
  #app
    height: 100%
    display flex
    flex-flow: column

  .nav
    display: flex
    position: relative
    width: 100%
    height: 40px
    flex-basis: 40px
    line-height: 40px
    border-1px(rgba(7, 17, 27, 0.1))
    .nav-item
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: 14px
        color: rgb(77, 85, 93)
        &.active
          color: rgb(240, 20, 20)

  .content
    flex 1

</style>
