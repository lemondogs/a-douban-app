<template>
    <div>
        <header class="m-header is-fixed is-bg top-search">
          <div class="search-wrap">
            <img src="../../assets/images/ic_search_gray.png" alt="">
            <span class="placeholder">影视 图片 唱片 小组 舞台剧等</span>
            <img src="../../assets/images/ic_scan_gray.png" alt="">
          </div>
          <div class="m-header-button is-right" style="text-align:center;width:50px">
            <a href="javascript:;"><img class="m-icon-img" src="../../assets/images/ic_chat_white.png" /></a>
          </div>
        </header>
        <div class="page-content">
            <m-swipe swipeid="swipe01" :autoplay="1000" paginationDirection="right">
            <div class="swiper-slide slide01" slot="swiper-con">slide 01</div>
            <div class="swiper-slide slide01" slot="swiper-con">slide 02</div>
            <div class="swiper-slide slide01" slot="swiper-con">slide 03</div>
            </m-swipe>
        </div>
        <m-cell title="提醒" icon>
        <img src="../../assets/images/ic_mine_notification.png" slot="icon">
        <a href="javascript:;" slot="cell-right"><img src="../../assets/images/ic_arrow_gray_small.png" alt=""></a>
        </m-cell>
        <m-cell title="设置">
        <a href="javascript:;" slot="cell-right"><img src="../../assets/images/ic_arrow_gray_small.png" alt=""></a>
        </m-cell>
        <div class="hot-wrap">
            <m-cell title="热门" label="hot">
                <!--<a href="javascript:;" slot="cell-right">更多<img src="../../assets/images/ic_arrow_gray_small.png" alt=""></a>-->
            </m-cell>
            <m-cell-media :author="item.target.author.name" :column="item.source_cn" :img="item.target.cover_url" v-for="(item,index) in hotData" :kry="item.id">
              <span slot="title">{{item.title}}</span>
              <span slot="describe">{{item.target.desc}}</span>
            </m-cell-media>
    </div>
</template>

<script>
    import mHeader from '../../components/header'
    import mSwipe from '../../components/swipe'
    import mCell from '../../components/cell'
    import mCellMedia from '../../components/cell-media'
    export default {
        name: 'index',
        components: {
            mHeader,
            mSwipe,
            mCell,
            mCellMedia
        }
        data(){
          return {
            recommendData: [],
            hotData: []
          }
        },

        created() {
          this.fetchData();
        },
        methods: {
          fetchData() {
            this.axios.get('/api/homeData').then((response) =>{
              let data = response.data.data.recommend_feeds;
              let recommend = [];
              let hot = [];
              for(var i in data) {
                if (data[i].card && data[i].card.name == '为你推荐') {
                  recommend.push(data[i]);
                } else {
                  hot.push(data[i]);
                }
              }

              this.recommendData = recommend;
              this.hotData = hot;
            })
          }
        }
    }
</script>


<style lang="less">
.is-fixed ~ .page-content{
padding-top:44px;
}
header.m-header {
    padding: 0 0 0 10px;
  }
  .top-search {
    .search-wrap {
      width: 100%;
      height: 30px;
      background: #fff;
      border-radius: 4px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      color: #c0c0c0;
      padding: 0 12px;
      .placeholder {
        flex: 1;
        text-align: left;
        padding-left: 12px;
      }
      img {
        width: 18px;
        height: 18px;
      }
    }
  }
  .hot-wrap,
  .recommend-wrap {
    padding-top: 12px;
  }
  .slide01{
    background:#41b883;
    text-align:center;
    line-height:200px;
    font-size:30px;
    color:#fff;
  }
  .slide02{
    background: #364a60;
    text-align: center;
    line-height: 200px;
    font-size: 30px;
    color: #fff;
  }
  .slide03{
    background: #ea6f5a;
    text-align: center;
    line-height: 200px;
    font-size: 30px;
    color: #fff;
</style>