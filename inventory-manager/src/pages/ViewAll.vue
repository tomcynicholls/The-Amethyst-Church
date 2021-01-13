<template lang="pug">

  .q-pa-md
    q-markup-table
      thead
        tr
          th ID Number
          th Type
          th Material
          th Shape
          th Colour
          th Weight (g)
          th Price (£)
      tbody
        tr( v-for="(item, id ) in stockData" )
          td {{ id }}
          td {{ item.type }}
          td {{ item.material }}
          td {{ item.shape }}
          td {{ formatColour(item.colour) }}
          td {{ item.weight || "-" }}
          td {{ formatPrice(item.price) }}

</template>

<script>
import stock from '../data/stock.json'

export default {
  name: 'ViewAll',
  data () {
    return {
      stockData: stock
    }
  },
  mounted () {
    console.log('got stock', this.stockData)
  },
  methods: {
    formatColour (colours) {
      // const result = ''

      // if (colours.length > 0) {
      //   colours.foreach()
      // }

      // console.log('colours', colours)

      // return colours.reduce((acc, curr) => {
      //   console.log('acc curr', acc, curr)
      //   acc += curr
      //   console.log('acc', acc)
      //   return acc
      // }, '')

      return colours.map((c) => { return c.charAt(0).toUpperCase() + c.slice(1) }).join(', ')
    },
    formatPrice (price) {
      return '£' + (price / 100).toFixed(2)
    }
  }
}
</script>

<style lang="stylus">

th,
td
  text-align left

</style>
