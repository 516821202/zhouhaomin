<template>
  <div id="app">
   <v-header :seller="seller"></v-header>
   <div class="tab">
     <div class="tab-tiem">
       <router-link to="/goods" >商品</router-link>
       </div>
     <div class="tab-tiem">
       <router-link to="/ratings">评价</router-link>
       </div>
     <div class="tab-tiem">
       <router-link to="/seller">商家</router-link>
       </div>  
   </div>
   <keep-alive>
    <router-view :seller="seller" ></router-view>  
   </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
import header from './components/header/header'
import {urlParse} from './common/js/util';

const ERR_OK=0;
export default {
  name:'app',
  data () {
    return {
     seller:{
       id:(()=>{
         let queryParam = urlParse();
          return queryParam.id;
       })()
     }
    };
  },
   components: {
    "v-header":header
  },
  created(){
    var _this=this;
    this.$axios.get('/api/seller?id'+this.seller.id).then(function(res){
      res=res.data;
      if(res.errno===ERR_OK){
        _this.seller=res.data;
        _this.seller= Object.assign({}, _this.seller, res.data);
      };
    });
  },
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"
  #app
    .tab
      display: flex
      width:100%
      height 40px
      line-height 40px
      border-bottom-1px(rgba(7,17,27,0.1))
      .tab-tiem
        flex:1
        text-align :center
        &>a
          display:block
          font-size:14px
          color:rgb(77,85,93)
          &.active
            color:rgb(240,20,20)
</style>
