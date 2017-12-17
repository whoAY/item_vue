<template>
  <!--页面顶部-->
  <div style="background: #f5f5f5">
    <header class="header">
      <div class="back iconfont">&#xe624;</div>
      <div class="search"><a href="#" class="prompt">输入城市/景点/游玩主题</a></div>
      <div class="city">城市</div>
    </header>
  <!--轮播图-->
  	<swiper :options="swiperOption" ref="mySwiper">
	    <swiper-slide v-for="item in swiperInfo" :key="item.id">
	      <div class="swiper-img-con">
	      	<img class="swiper-img" :src="item.imgUrl">
	      </div>
	    </swiper-slide>
	    <div class="swiper-pagination"  slot="pagination"></div>
  	</swiper>
  <!--小轮播图-->
    <swiper :options="option" style="background: #fff;height: 3.8rem;border-bottom: 1px solid #e1e1e1">
      <swiper-slide v-for="(pageInfo, index) in pages" :key="index">
        <div class="icon-wrapper">
          <div v-for="item in pageInfo" :key="item.id" class="icon-item"  style="margin-top: .4rem;">
            <dl>
              <dt class="icon-img-con">
                <img class="icon-img" :src="item.imgUrl">
              </dt>
              <dd class="icon-message">{{item.message}}</dd>
            </dl>
          </div>
        </div>
      </swiper-slide>
      <div class="swiper-pagination"  slot="pagination" style="position: relative;"></div>
    </swiper>
  <!--定位-->
    <ul class="listitem-con">
      <li class="listentrance-item" style="border-right: 1px solid #e1e1e1">
        <span class="iconfont">&#xe611;</span>
        定位失败
      </li>
      <li class="listentrance-item">
        <span class="iconfont">&#xe74d;</span>
        五折泡温泉
      </li>
    </ul>
  <!--广告-->
    <ul class="mp-activity-con">
      <li class="mp-activity-item" style="border-right: 1px solid #e1e1e1;background: url(http://img1.qunarzz.com/piao/fusion/1710/a2/e395615b16fb1302.png) center center no-repeat;
    background-size: auto 100%;"></li>
      <li class="mp-activity-item" style="background: url(http://img1.qunarzz.com/piao/fusion/1711/8a/4c62d1a89fc2d602.png) center center no-repeat;
    background-size: auto 100%;"></li>
    </ul>
    <!--热销推荐-->
    <div class="lazy-load">
      <h2 class="mp-modtitle">热销推荐</h2>
      <ul class="mp-list">
        <li class="mp-hot-prod" v-for="list in listInfo" :key="list.id">
          <div class="mp-hotlist-img">
            <img :src="list.imgUrl">
          </div>
          <div class="mp-hotlist-infos">
            <div class="mp-hotlist-title">{{list.title}}</div>
            <div class="mp-hotlist-message">{{list.message}}</div>
          </div>
          <div class="mp-hotlist-price">
            ¥
            <i class="mp-price-num">{{list.price}}</i>
            <span class="mp-price-text">起</span>
          </div>
        </li>
      </ul>
      <div class="mp-modmore">
        <a href="#">查看所有产品</a>
      </div>
    </div>
    <!--周末去那儿-->
    <div>
      <h2 class="mp-modtitle">周末去那儿</h2>
      <div style="background: #fff">
        <div class="mp-product-item" v-for="week in weekInfo" :key="week.id">
          <a href="#">
            <div class="product-imgcontainer">
              <img :src="week.imgUrl">
            </div>
            <div class="mp-product-info">
              <p class="product-title">{{week.title}}</p>
              <p class="product-message">{{week.message}}</p>
            </div>
          </a>
        </div>
      </div>
      <div class="mp-price-message">
        <div class="mp-price-messageinfo">
          <span class="mp-price-piao">！票面价</span>
          是指通过景区指定窗口售卖的纸质门票上标注的价格
        </div>
      </div>
    </div>
    <!--页面底部-->
    <div class="footer">
      <div class="footer-nav">
        <ul class="footer-main-nav">
          <li class="flight">
            <a href="#">
              <div class="icon"></div>
              <span class="title">机票</span>
            </a>
          </li>
          <li class="hotel">
            <a href="#">
              <div class="icon"></div>
              <span class="title">酒店</span>
            </a>
          </li>
          <li class="gongyu">
            <a href="#">
              <div class="icon"></div>
              <span class="title">公寓</span>
            </a>
          </li>
          <li class="more">
            <div class="iconfont jiantou">&#xe607;</div>
            <span>更多</span>
          </li>
        </ul>
      </div>

      <ul class="footer_nav clearfix">
        <li><a href="#">登录</a></li>
        <li><a href="#">我的订单</a></li>
        <li><a href="#">最近浏览</a></li>
        <li><a href="#">关于我们</a></li>
      </ul>
      
      <ul class="moblie_pc">
        <li><a href="#">触屏版</a></li>
        <li><a href="#">电脑版</a></li>
      </ul>

      <div class="copyright">
        <span>Qunar 京ICP备05021087</span>
        <a href="#" class="idea">意见反馈</a>
      </div>

    </div>

  </div>
</template>

<script>
export default {
  name: 'Index',
  data () {
    return {
      swiperInfo: [],
      iconInfo: [],
      listInfo: [],
      weekInfo: [],
      swiperOption: {
        autoplay: 1000,
        direction: 'horizontal',
        pagination: '.swiper-pagination',
        loop: true
      },
      option: {
        direction: 'horizontal',
        pagination: '.swiper-pagination'
      }
    }
  },

  computed: {
    pages () {
      const pages = []
      this.iconInfo.forEach((value, index) => {
        let page = Math.floor(index / 8)
        if (!pages[page]) {
          pages[page] = []
        }
        pages[page].push(value)
      })
      return pages
    }
  },

  methods: {
    getIndexData () {
      this.$http.get('/static/index.json')
        .then(this.handleGetDataSucc.bind(this))
    },
    handleGetDataSucc (res) {
      const body = res.body
      if (body && body.data && body.data.swiper) {
        this.swiperInfo = res.body.data.swiper
        this.iconInfo = body.data.icons
        this.listInfo = body.data.list
        this.weekInfo = body.data.weekend
      }
    }
  },

  created () {
    this.getIndexData()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .header {
    display: flex;
    justify-content: space-around;
    align-items: center;
    background: #05bad5;
    color: #fff;
  }
  .back {
    box-sizing: border-box;
    width: .6rem;
    height: .88rem;
    font-size: .16rem;
    text-align: center;
    padding: .3rem .2rem;
  }
  .search{
    flex: 1;
  }
  .prompt {
    display: block;
    height: .6rem;
    color: #ccc;
    background: #fff;
    line-height: 0.6rem;
    font-size: 0.26rem;
    padding-left: .2rem;
    border-radius: .1rem; 
  }
  .city {
    box-sizing: border-box;
    width: 1.2rem;
    line-height: .86rem;
    text-align: left;
    padding: 0 .2rem;
  }
  .city::after{
    content:"";
    display: block;
    width: 0px;
    height: 0px;
    border: 0.1rem solid #fff;
    border-color: white transparent transparent transparent;
    position: absolute;
    right: 0.3rem;
    top: 0.36rem;
  }
  .swiper-img-con {
    overflow: hidden;
    width: 100%;
	height: 0;
	padding-bottom: 31.25%; 
   }
  .swiper-img {
	width: 100%;
  }
  .icon-wrapper {
    display: flex;
    flex-wrap: wrap;
    text-align: center;
  }
  .icon-item {
    float: left;
    width: 25%;
    height: 1.4rem;
    box-sizing: border-box;
    display: flex;
    justify-content: center;
    position: relative;
  }
  .icon-img-con {
    height: .66rem;
  }
  .icon-img {
    width: .66rem;
    height: .66rem;
  }
  .swiper-pagination {
    bottom: .3rem;
  }
  .icon-message {
    padding-top: .2rem;
    color: #212121;
    font-size: .28rem;
  }
  .listitem-con {
    width: 100%;
    height: .98rem;
    background-color: #fff;
  }
  .listentrance-item {
    float: left;
    width: 50%;
    height: .98rem;
    font-size: .28rem;
    color: #212121;
    line-height: .98rem;
    text-align: center;
    box-sizing: border-box;
  }
  .mp-activity-con {
    width: 100%;
    margin-top: .2rem;
    background-color: #fff;
    height: 1.4rem;
  }
  .mp-activity-item {
    float: left;
    width: 50%;
    height: 1.4rem;
    line-height: 1.4rem;
    color: #212121;
    font-size: .28rem;
    box-sizing: border-box;
  }
  .mp-modtitle {
    height: .8rem;
    padding-left: .26rem;
    line-height: .8rem;
    color: #212121;
  }
  .mp-list {
    background: #fff;
  }
  .mp-hot-prod {
    position: relative;
    border-bottom: 1px solid #f1f1f1;
    overflow: hidden;
    height: 1.4rem;
    padding: .24rem;
  }
  .mp-hotlist-img {
    position: absolute;
    top: .24rem;
    left: .24rem;
    width: 1.4rem;
    height: 1.4rem;
  }
  .mp-hotlist-img img {
    vertical-align: top;
    transition: opacity 400ms;
    width: 1.4rem;
    height: 1.4rem;
  }
  .mp-hotlist-infos {
    margin-left: 1.6rem;
  }
  .mp-hotlist-title {
    overflow: hidden;
    margin-top: .04rem;
    margin-bottom: .1rem;
    color: #212121;
    font-size: .3rem;
  }
  .mp-hotlist-message {
    overflow: hidden;
    margin-bottom: .1rem;
    height: .4rem;
    line-height: .4rem;
    color: #9e9e9e;
  }
  .mp-hotlist-price {
    margin-left: 1.6rem;
    color: #ff8300;
    font-size: .24rem;
  }
  .mp-price-num {
    font-size: .36rem;
    padding: 0 .04rem;
  }
  .mp-price-text {
    color: #9e9e9e;
    font-size: .24rem;
  }
  .mp-modmore {
    height: .88rem;
    color: #00afc7;
    line-height: .88rem;
    text-align: center;
    margin-top: -.02rem;
    background: #fff;
  }
  .mp-modmore a{
    display: block;
    color: #00afc7;
    text-decoration: none;
  }
  .product-imgcontainer {
    overflow: hidden;
    height: 0;
    padding-bottom: 37.4375%;
  }
  .product-imgcontainer img {
    width: 100%;
  }
  .mp-product-info {
    position: relative;
    padding: .14rem .2rem .2rem .2rem;
  }
  .product-title {
    overflow: hidden;
    padding-right: 1.4rem;
    color: #212121;
    font-size: .28rem;
    line-height: .48rem;
  }
  .product-message {
    overflow: hidden;
    padding-right: 1.4rem;
    color: #616161;
    font-size: .24rem;
    line-height: .42rem;
  }
  .mp-price-message {
    margin-top: .1rem;
    padding: .14rem .1rem;
    font-size: .24rem;
    line-height: .32rem;
    background: #fff;
    color: #616161;
  }
  .mp-price-messageinfo {
    margin-left: .3rem;
  }
  .mp-price-piao {
    font-weight: bold;
  }
  .footer {
    background: #f3f3f3;
  }
  .footer-main-nav {
    display: flex;
    justify-content: space-around;
    align-items: center;
  }
  .footer-main-nav li {
    display: flex;
    align-items: center;
    padding-left: 10px;
    height: .8rem;
    background: none;
  }
  .footer-main-nav li .icon {
    float: left;
    width: 22px;
    height: 22px;
    background: url(//source.qunarzz.com/site/images/wap/touch/images/v2/images2x/nav_7.png) 0 0 no-repeat;
    background-size: 175px 50px;
  }
  .footer-main-nav li .title {
    display: inline-block;
    color: #9e9e9e;
    margin: 3px 0 0 5px;
    border-bottom: 1px solid #acacac;
    font-size: 12px;
  }
  .footer-main-nav .hotel .icon {
    background-position: -25px 0px;
  }
  .footer-main-nav .gongyu .icon {
    background-position: -150px 0px;
  }
  .footer-main-nav .more {
    position: relative;
    top: -4px;
    right: 6px;
    background: #f3f3f3;
    font-size: 14px;
    color: #9e9e9e;
    width: 70px;
  }
  .footer-main-nav .more span {
    display: inline-block;
    border-left: 1px solid #acacac;
    padding-left: 30px;
    margin-top: 5px;
    font-size: 14px;
    color: #9e9e9e;
  }
  .footer-main-nav .more .jiantou {
    position: absolute;
    bottom: 10px;
    left: 20px;
  }
  .footer_nav {
    width: 100%;
    height: 39px;
    border-bottom: 1px solid #cacaca;
  }
  .footer_nav li {
    float: left;
    padding-left: 20px;
  }
  .footer_nav li a {
    position: relative;
    color: #25a4bb;
    font-size: 14px;
    line-height: 39px;
  }
  .moblie_pc {
    padding: 10px 0 0 0;
    text-align: center;
  }
  .moblie_pc li {
    display: inline-block;
    margin: 0 15px;
  }
  .copyright {
    color: #9e9e9e;
    text-align: center;
    font-size: 14px;
    padding: 10px;
  }
  .copyright .idea {
    color: #9e9e9e;
    height: 33px;
  }
</style>
