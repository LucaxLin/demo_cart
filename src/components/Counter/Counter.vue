<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{count}}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import bus from '../eventBus';
export default {
  data(){
    return {
      count: this.goods_count
    }
  },
  props:{
    goods_count: {
      default:0,
      type:Number
    },
    goods_id:{
      required: true,
      type: Number
    }
  },
  methods: {
    sub(){
      if (this.count>1) {
        this.count-=1
        bus.$emit('countChange',{count: this.count,id: this.goods_id})
        return this.count
      }
    },
    add(){
      this.count+=1
      bus.$emit('countChange',{count: this.count,id: this.goods_id})
      return this.count
    },
  },
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
