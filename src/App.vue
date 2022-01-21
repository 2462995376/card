<template>
  <div class="app-container">
    <Header></Header>
    <Goods
      v-for="item in list"
      :key="item.id"
      :name="item.goods_name"
      :price="item.goods_price"
      :state="item.goods_state"
      :img="item.goods_img"
      :id="item.id"
      @state-change="newState"
      @numberCount="numberCount"
    ></Goods>
    <Footer :count="this.countPrice" :amount="this.amount" @fullStateChange="fullStateChange" :fullState="this.fullState"></Footer>
  </div>
</template>

<script>
import Header from '@/components/Header/Header.vue'
import Goods from '@/components/Goods/Goods.vue'
import axios from 'axios'
import Footer from '@/components/Footer/Footer.vue'
export default {
  data() {
    return {
      list: [],
      cbFull: false
    }
  },
  created() {
    this.initCarCle()
    this.list.push({ goods_count: 1 })
  },
  methods: {
    async initCarCle() {
      const { data: res } = await axios.get('https://www.escook.cn/api/cart')
      if (res.status === 200) this.list = res.list
    },
    newState(e) {
      this.list.filter((item) => {
        if (item.id === e.id) item.goods_state = e.state
      })
    },
    numberCount(e) {
      this.list.filter((item) => {
        if (item.id === e.id) item.goods_count = e.number
      })
    },
    fullStateChange(e) {
      this.list.forEach((item) => (item.goods_state = e))
    }
  },
  computed: {
    fullState() {
      return this.list.every((item) => item.goods_state === true)
    },
    countPrice() {
      return this.list
        .filter((item) => item.goods_state === true)
        .reduce((amt, item) => {
          return (amt += item.goods_price * item.goods_count)
        }, 0)
    },
    amount() {
      let amount = 0
      this.list
        .filter((item) => item.goods_state === true)
        .forEach((item) => {
          amount += item.goods_count
        })
      return amount
    }
  },
  components: {
    Header,
    Goods,
    Footer
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
