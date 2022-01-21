<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click.trim="numberReduce" ref="Reduce">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ this.number }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click.trim="numberAdd">+</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      number: 1
    }
  },
  props: {
    id: {
      type: Number
    }
  },
  methods: {
    numberAdd() {
      this.number++
      this.$emit('numberChange', { number: this.number, id: this.id })
    },
    numberReduce() {
      if (this.number <= 1) {
        this.number = 1
        this.$refs.Reduce.style = { disabled: false }
        return
      } else {
        this.$refs.Reduce.style = { disabled: true }
      }
      this.number--
      this.$emit('numberChange', { number: this.number, id: this.id })
    }
  }
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
