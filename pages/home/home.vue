<template>
  <view>
    <view class="TopSearch">
      <view class="SearchBtn">
        宇哥
      </view>
    </view>
    <view>
      <!-- 轮播图区域 -->
      <swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
        <!-- 循环渲染轮播图的 item 项 -->
        <!-- 循环渲染轮播图的 item 项 -->
        <swiper-item v-for="(item, i) in banners" :key="i">
          <view class="swiper-item">
            <!-- 动态绑定图片的 src 属性 -->
            <image @click="goDetail(item.goods_id)" :src="item.image_src"></image>
          </view>
        </swiper-item>
      </swiper>
    </view>
    <!-- 分类导航区域 -->
    <view class="nav-list">
      <view class="nav-item" @click="navClickHandler(item)" v-for="(item, i) in navs" :key="i">
        <image :src="item.image_src" class="nav-img"></image>
      </view>
    </view>
    <!-- 楼层区域 -->
    <view class="floor-list">
      <!-- 楼层 item 项 -->
      <view class="floor-item" v-for="(item, i) in floors" :key="i">
        <!-- 楼层标题 -->
        <image :src="item.floor_title.image_src" class="floor-title"></image>
        <!-- 楼层图片区域 -->
        <view class="floor-img-box">
          <!-- 左侧大图片的盒子 -->
          <view class="left-img-box">
            <image class="left-img" @click='goGoodsList(item.product_list[0].navigator_url)'
              :src="item.product_list[0].image_src" :style="{width: item.product_list[0].image_width + 'rpx'}"></image>
          </view>
          <!-- 右侧 4 个小图片的盒子 -->
          <view class="right-img-box">
            <view @click='goGoodsList(item2.navigator_url)' class="right-img-item"
              v-for="(item2, i2) in item.product_list" :key="i2" v-if="i2 !== 0">
              <image :src="item2.image_src" mode="widthFix" :style="{width: item2.image_width + 'rpx'}"></image>
            </view>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
  import {
    getBanners,
    getNavList,
    getFloorList
  } from '@/api/home.js'
  export default {
    data() {
      return {
        banners: [],
        navs: [],
        floors: []
      };
    },
    methods: {
      //楼层点击
      goGoodsList(url) {
        console.log(url)
        uni.navigateTo({
          url: '/subpkg/goods_list/goods_list?' + url.split('?')[1]
        })

      },
      //楼层数据获得
      async getFloorList() {
        const res = await getFloorList()
        this.floors = res.message
        console.log('楼层数据', this.floors)
      },
      // nav-item 项被点击时候的事件处理函数
      navClickHandler(item) {
        // 判断点击的是哪个 nav
        if (item.name === '分类') {
          uni.switchTab({
            url: '/pages/cate/cate'
          })
        }
      },
      // 加载导航列表数据
      async loadNavList() {
        const res = await getNavList()
        console.log(res)
        this.navs = res.message
      },
      // 跳转商品详情
      goDetail(id) {
        uni.navigateTo({
          url: '/subpkg/goods_detail/goods_detail?goods_id=' + id
        })
      },
      async loadBanners() {
        const res = await getBanners()
        this.banners = res.message
        console.log(res)
      }

    },
    onLoad() {
      this.loadBanners()
      this.loadNavList()
      this.getFloorList()
    }
  }
</script>

<style lang="scss">
  .TopSearch {
    width: 750rpx;
    height: 70px;
    background-color: #FFC0CB;
    display: flex;
    justify-content: center;
    overflow: hidden;
  }

  .SearchBtn {
    border-radius: 10px;
    width: 660rpx;
    height: 30px;
    margin-top: 30px;
    background-color: #FF69B4;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  swiper {

    height: 330rpx;

    .swiper-item,
    image {
      width: 100%;
      height: 100%;
    }
  }

  .nav-list {
    display: flex;
    justify-content: space-around;
    margin: 15px 0;

    .nav-img {
      width: 128rpx;
      height: 140rpx;
    }
  }

  .floor-title {
    height: 60rpx;
    width: 100%;
    display: flex;
  }

  .right-img-box {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  }

  .floor-img-box {
    display: flex;
    padding-left: 10rpx;

    .left-img {
      height: 392rpx !important;
    }
  }
</style>
