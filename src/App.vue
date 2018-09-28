<template>
  <div id="app">
    <img alt="logo" src="./assets/logo.png" style="height: 80px">
    <h1>Table Man</h1>
    <p>共 {{ headers.length }} 列，{{ dataset.length }} 行数据</p>
    <table-man
      :dataset="dataset"
      :headers="headers"
      :fixed="[0, 2]"
      width="800px"
      height="520px"
      custom-wrap-class="table-container"
      custom-cell-class="table-cell"
    >
    </table-man>
    <button @click="randomUpdate">更新数据 (1000 条以内)</button>
  </div>
</template>

<script>
import TableMan from './components/TableMan'

export default {
  name: 'app',
  data() {
    let dataset = []
    let headers = []
    for (let i = 0; i < 10; ++i) {
      headers.push('col ' + i)
    }
    return { dataset, headers }
  },
  mounted() {
    this.randomUpdate()
  },
  methods: {
    randomUpdate() {
      let dy = []
      let num = ~~(Math.random() * 1000)
      for (let i = 0; i < num; ++i) {
        dy.push(this.headers.map(() => 'cell ' + i))
      }
      this.dataset = dy
    }
  },
  components: {
    TableMan
  }
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 30px
.table-container
  position relative
  margin 16px auto
</style>
