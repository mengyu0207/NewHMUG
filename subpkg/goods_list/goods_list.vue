<template>
  <view>
    商品列表
    <van-button>123</van-button>
    <van-button>hhhhh </van-button>
    <van-card :thumb-link="`/subpkg/goods_detail/goods_detail?id=${item.goods_id}`" :thumb="item.goods_small_logo"
      v-for="item in goodsList" :key="item.goods_id" :price="item.goods_price|toFixed" :title="item.goods_name">
    </van-card>
  </view>
</template>

<script>
  import {
    getSearchGoods
  } from '@/api/goods.js'
  export default {
    data() {
      return {
        isloading: false,
        // 商品列表的数据
        goodsList: [],
        // 总数量，用来实现分页
        total: 0,
        // 请求参数对象
        queryObj: {
          // 查询关键词
          query: '',
          // 商品分类Id
          cid: '',
          // 页码值
          pagenum: 1,
          // 每页显示多少条数据
          pagesize: 10
        }
      }
    },
    methods: {
      async loadGoods() {
        // ** 打开节流阀
        this.isloading = true
        const res = await getSearchGoods(this.queryObj)
        console.log(res)
        // ** 关闭节流阀
        this.isloading = false
        this.goodsList = [...this.goodsList, ...res.message.goods]
        this.total = res.message.total
        //
        uni.stopPullDownRefresh()
      }
    },
    onLoad(options) {
      // 将页面参数转存到 this.queryObj 对象中
      this.queryObj.query = options.query || ''
      this.queryObj.cid = options.cid || ''
      this.loadGoods()
    },
    // 下拉刷新的事件
    onPullDownRefresh() {
      // 1. 重置关键数据
      this.queryObj.pagenum = 1
      this.total = 0
      this.isloading = false
      this.goodsList = []

      // 2. 重新发起请求
      this.loadGoods()
    },
    onReachBottom() {
        if (this.queryObj.pagenum * this.queryObj.pagesize >= this.total) return toast('数据加载完毕！')
      if (this.isloading) return
      // 让页码值自增 +1
      this.queryObj.pagenum += 1
      // 重新获取列表数据
      this.loadGoods()
    }
  }
</script>

<style lang="scss">

</style>
