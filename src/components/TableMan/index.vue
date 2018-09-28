<template>
  <div class="table-man--wrap" ref="wrap" :class="customWrapClass" :style="{width: width || '960px', height: height || '512px'}">
    <table class="table-man--table">
      <tr class="table-man--head" :style="{ top: scrollTop + 'px' }">
        <th v-if="indexed" class="table-man--cell table-man--cell-indexed" :class="customCellClass">#</th>
        <th class="table-man--cell" :class="customCellClass" v-for="(col, index) in headers" v-if="fixeds.indexOf(index) > -1" :key="index">{{ col }}</th>
        <th class="table-man--cell" :class="customCellClass" v-for="(col, index) in headers" v-if="fixeds.indexOf(index) == -1" :key="index">{{ col }}</th>
      </tr>
      <tr class="table-man--head-guest">
        <th v-if="indexed" class="table-man--cell table-man--cell-indexed" :class="customCellClass">#</th>
        <th class="table-man--cell" :class="customCellClass" v-for="(col, index) in headers" v-if="fixeds.indexOf(index) > -1" :key="index">{{ col }}</th>
        <th class="table-man--cell" :class="customCellClass" v-for="(col, index) in headers" v-if="fixeds.indexOf(index) == -1" :key="index">{{ col }}</th>
      </tr>
      <tr v-for="(row, index) in data" :key="index">
        <td v-if="indexed" class="table-man--cell table-man--cell-indexed" :class="customCellClass">{{ index }}</td>
        <td class="table-man--cell" :class="customCellClass" v-for="(col, index) in row" v-if="fixeds.indexOf(index) > -1" :key="index">{{ col }}</td>
        <td class="table-man--cell" :class="customCellClass" v-for="(col, index) in row" v-if="fixeds.indexOf(index) == -1" :key="index">{{ col }}</td>
      </tr>
    </table>

    <table v-if="fixeds.length > 0" class="table-man--table table-man--table-guest" :class="scrollLeft>0?'table-man--fixed-fade':''" :style="{ left: scrollLeft + 'px' }">
      <tr class="table-man--head" :style="{ top: scrollTop + 'px' }">
        <th v-if="indexed" class="table-man--cell table-man--cell-indexed" :class="customCellClass">#</th>
        <th v-for="(col, index) in headers" v-if="fixeds.indexOf(index) > -1" class="table-man--cell" :class="customCellClass" :key="index">{{ col }}</th>
      </tr>
      <tr class="table-man--head-guest">
        <th v-if="indexed" class="table-man--cell table-man--cell-indexed" :class="customCellClass">#</th>
        <th v-for="(col, index) in fixeds" v-if="fixeds.indexOf(index) > -1" class="table-man--cell" :class="customCellClass" :key="index">{{ col }}</th>
      </tr>
      <tr v-for="(row, index) in data" :key="index">
        <td v-if="indexed" class="table-man--cell table-man--cell-indexed" :class="customCellClass">{{ index }}</td>
        <td v-for="(col, index) in row" v-if="fixeds.indexOf(index) > -1" class="table-man--cell" :class="customCellClass" :key="index">{{ col }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
function isMetric(val) {
  return /^[0-9.]*[0-9]+(px|em|rem|vh|vw)?$/.test(val)
}
function isPositiveIntegerONEorSET(val) {
  return (
    (val instanceof Array && val.every(i => /^[0-9]+$/.test(i))) ||
    /^[0-9]+$/.test(val)
  )
}

export default {
  name: 'TableMan',
  props: {
    dataset: { type: Array, required: true },
    headers: { type: Array, required: true },
    fixed: {
      validator: isPositiveIntegerONEorSET,
      default() {
        return []
      }
    },
    width: { validator: isMetric },
    height: { validator: isMetric },
    customCellClass: String,
    customWrapClass: String,
    indexed: { type: Boolean, default: true }
  },
  data() {
    return {
      data: Object.freeze(this.dataset),
      scrollTop: 0,
      scrollLeft: 0
    }
  },
  computed: {
    fixeds() {
      if (this.fixed instanceof Array) return this.fixed
      return [this.fixed]
    }
  },
  watch: {
    dataset(val) {
      this.data = Object.freeze(val)
    }
  },
  mounted() {
    this.$refs['wrap'].addEventListener('scroll', ev => {
      this.scrollTop = ev.target.scrollTop
      this.scrollLeft = ev.target.scrollLeft
    })
  }
}
</script>

<style lang="stylus">
.table-man--wrap
  background #e3f2fd
  overflow-x scroll
  .table-man--table
    display table
    border-collapse collapse
    border-spacing 0
    tr
      background #e3f2fd
    tr:nth-child(2n)
      background #e8f5e9
    .table-man--head
      position absolute
      background #b2dfdb
    .table-man--head-guest
      visibility hidden
    .table-man--cell
      min-width 100px
      line-height 34px
    .table-man--cell-indexed
      min-width 50px
  .table-man--table-guest
    position absolute
    top 0
  .table-man--fixed-fade
    box-shadow 0px 0px 4px 0px #0000004d
</style>
