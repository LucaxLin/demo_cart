<template>
  <div class="app-container">
    <Header :title="'购物车案例'"></Header>
    <goods
      v-for="item in cartList"
      :key="item.id"
      :title="item.goods_name"
      :pic="item.goods_img"
      :price="item.goods_price"
      :state="item.goods_state"
      :count="item.goods_count"
      :id="item.id"
      @stateChange="getNewState"
    ></goods>
    <Footer
      :checkAll="fullState"
      @fullChange="GetFullChange"
      :totalPrice="totalPrice"
      :totalCount="totalCount"
    ></Footer>
  </div>
</template>

<script>
import Header from "@/components/Header/Header.vue";
import Goods from "./components/Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";
import axios from "axios";
import bus from "./components/eventBus";
export default {
  data() {
    return {
      cartList: [],
    };
  },
  methods: {
    async initCarList() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      return (this.cartList = res.list);
    },
    getNewState(val) {
      this.cartList.some((item) => {
        if (item.id === val.id) {
          item.goods_state = val.state;
          return true;
        }
      });
    },
    GetFullChange(val) {
      this.cartList.forEach((element) => {
        element.goods_state = val;
      });
    },
  },
  components: {
    Header,
    Goods,
    Footer,
  },
  created() {
    this.initCarList();
    bus.$on("countChange", (val) => {
      this.cartList.some((item) => {
        if (item.id === val.id) {
          item.goods_count = val.count;
          return true;
        }
      });
    });
  },
  computed: {
    fullState() {
      return this.cartList.every((item) => item.goods_state);
    },
    totalPrice() {
      return this.cartList
        .filter((item) => item.goods_state)
        .reduce((total, item) => {
          return (total += item.goods_price * item.goods_count);
        }, 0);
    },
    totalCount() {
      return this.cartList
        .filter((item) => item.goods_state)
        .reduce((total, item) => {
          return (total += item.goods_count);
        }, 0);
    },
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
