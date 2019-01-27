<template>
  <div>
    <div class="index-container">
      <div class="search-container">
        <input type="text" placeholder="搜索">
        <icon type="search" size="12"></icon>
      </div>
      <div class="swiper-container">
        <swiper class="item" indicator-dots autoplay circular indicator-active-color="#0094ff">
          <swiper-item v-for="item in swiperList" :key="item.image_src">
            <img mode="aspectFill" :src="item.image_src">
          </swiper-item>
        </swiper>
      </div>
      <div class="nav-container">
        <div class="nav" v-for="item in navList">
          <img :src="item.image_src" alt>
          <p>{{item.name}}</p>
        </div>
      </div>
      <div class="floor-container">
        <div class="floor" v-for="item in floorList">
          <div class="title">
            <img :src="item.floor_title.image_src" alt>
            <h3>{{item.floor_title.name}}</h3>
          </div>
          <div class="bottom">
            <img v-for="(it, i) in item.product_list" :src="it.image_src" alt>
          </div>
        </div>
      </div>
      <div class="bottom-line">
        <i class="iconfont icon-xiao"></i>
        我是有底线的
      </div>
      <div class="tTop" @click='tTop' v-show="isShow">
        <i class="iconfont icon-jiantoushang"></i>
        顶部
      </div>
    </div>
  </div>
</template>

<script>
import hxios from "../../utils/index.js";
export default {
  data() {
    return {
      swiperList: [],
      //nav 的数据
      navList: [],
      //楼层数据
      floorList: [],
      //返回顶部
      isShow: false
    };
  },

  async created() {
    // let resSwiper = await hxios.get({
    //   url: "api/public/v1/home/swiperdata"
    // });
    // // console.log(resSwiper);

    // this.swiperList = resSwiper.data.message;
    // let resList = await hxios.get({
    //   url: "api/public/v1/home/catitems"
    // });
    // // console.log(resList);
    // this.navList = resList.data.message;

    let p1 = await hxios.get({
      url: "api/public/v1/home/swiperdata"
    });
    let p2 = await hxios.get({
      url: "api/public/v1/home/catitems"
    });
    let p3 = await hxios.get({
      url: "api/public/v1/home/floordata"
    });

    let totalRes = await Promise.all([p1, p2, p3]);
    // console.log(totalRes);

    this.swiperList = totalRes[0].data.message;
    this.navList = totalRes[1].data.message;
    this.floorList = totalRes[2].data.message;
  },
  // 滚动事件
  onPageScroll(event) {
    // console.log(event);
    if (event.scrollTop > 170) {
      this.isShow = true;
    } else {
      this.isShow = false;
    }
  },
  methods: {
    // 返回顶部
    tTop() {
      wx.pageScrollTo({
        scrollTop: 0, //滚动到页面的目标位置（单位px）,
        duration: 300 //滚动动画的时长，默认300ms，单位 ms,
      });
    }
  }
};
</script>

<style lang="scss">
$uRed: #ff2219;
.index-container {
  .search-container {
    padding: 20rpx 16rpx;
    box-sizing: border-box;
    width: 100%;
    background-color: $uRed;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 99;
    input {
      background-color: #fff;
      height: 60rpx;
      padding-left: 376rpx;
      display: block;
      border-radius: 10rpx;
      width: 100%;
      box-sizing: border-box;
      font-size: 24rpx;
    }
    icon {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
    }
  }
  .swiper-container {
    margin-top: 100rpx;

    .item {
      box-sizing: border-box;
      width: 100%;
      swiper-item {
        img {
          display: block;
          width: 100%;
        }
      }
    }
  }
  .nav-container {
    display: flex;
    padding-top: 24rpx;
    padding-bottom: 29rpx;
    background-color: #fff;
    box-sizing: border-box;
    .nav {
      flex: 1;
      text-align: center;
      align-items: center;
      img {
        display: block;
        width: 128rpx;
        height: 128rpx;
        margin: 0 auto;
      }
      p {
        font-size: 24rpx;
        color: #333;
        margin-top: 10rpx;
      }
    }
  }
  .floor-container {
    .floor {
      .title {
        box-sizing: border-box;
        position: relative;
        height: 90rpx;
        padding-top: 30rpx;
        padding-bottom: 20rpx;
        padding-left: 15rpx;
        img {
          position: absolute;
          height: 90rpx;
          left: 0;
          top: 0;
          display: block;
          width: 100%;
        }
        h3 {
          color: #ff7b94;
          position: absolute;
          left: 30rpx;
          top: 50%;
          transform: translateY(-50%);
        }
      }
      .bottom {
        margin: 20rpx 0 0 16rpx;
        overflow: hidden;
        img {
          display: block;
          width: 33.333%;
          float: left;
          width: 230rpx;
          height: 190rpx;
          margin-right: 10rpx;
          &:first-child {
            width: 230rpx;
            height: 390rpx;
          }
          &:nth-child(2) {
            margin-bottom: 10rpx;
          }
          &:nth-child(3) {
            margin-bottom: 10rpx;
          }
        }
      }
    }
  }
  .bottom-line {
    font-size: 24rpx;
    display: flex;
    color: #999;
    justify-content: center;
    margin-top: 20rpx;
  }
  .tTop {
    position: fixed;
    bottom: 15rpx;
    right: 15rpx;
    text-align: center;
    width: 90rpx;
    height: 90rpx;
    border-radius: 50%;
    background: #ccc;
    font-size: 26rpx;
  }
}
</style>
