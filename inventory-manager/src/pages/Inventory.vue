<template lang="pug">

  .q-pa-md
    q-table(
      :data="processedStock",
      :columns="columns"
      row-key="id"
      :filter="filter"
      :filter-method="filterMethod"
      :pagination="pagination"
    )
      template(v-slot:top="props")
        q-select.q-ma-md(
          label="Available"
          outlined
          v-model="availableFilter"
          :options="availableOptions"
          emit-value
          map-options
        )
        q-select.q-ma-md.type-select(
          label="Type"
          outlined
          v-model="typeFilter"
          :options="typeOptions"
        )
        q-btn.q-mx-md(
          label="Add New"
          color="primary"
          @click="showAddNew()"
        )
      template(v-slot:body="props")
        q-tr(
          :props="props"
          :class="{unavailable: !props.row.available}"
        )
          q-td(v-for="col in props.cols" :key="col.name" :props="props")
            div(v-if="col.name==='actions'")
              q-btn(
                flat
                icon="edit"
                color="primary"
                @click="showEditProduct(props.row)"
              )
              q-btn(
                flat
                :icon="props.row.available ? 'check_box' : 'check_box_outline_blank'"
                color="primary"
                @click="toggleAvailable(props.row)"
              )
            div(v-else) {{ col.value }}

    q-dialog(
      v-model="flags.showProductView"
      persistent
      transition-show="flip-down"
      transition-hide="flip-up"
    )
      productView(
        v-if="selectedProduct"
        title="Update"
        :product="selectedProduct"
        @save="updateProduct"
        @close="clearProductView"
      )
      productView(
        v-else
        title="Add New"
        @save="addNewProduct"
        @close="clearProductView"
      )
</template>

<script>
import stock from '../data/stock.json'
import productView from '../components/Product.vue'

export default {
  name: 'ViewAll',
  components: {
    productView
  },
  data () {
    return {
      columns: [
        {
          name: 'id',
          required: true,
          label: 'ID Number',
          align: 'left',
          field: row => row.id,
          sortable: true
        },
        { name: 'type', label: 'Type', field: 'type', align: 'left', sortable: true },
        { name: 'material', label: 'Material', field: 'material', align: 'left', sortable: true },
        { name: 'shape', label: 'Shape', field: 'shape', align: 'left' },
        { name: 'colour', label: 'Colour(s)', field: 'colour', align: 'left', format: (val) => this.formatColour(val) },
        { name: 'weight', label: 'Weight (g)', field: 'weight', align: 'left' },
        { name: 'price', label: 'Price (£)', field: 'price', align: 'left', sortable: true, format: (val) => this.formatPrice(val) },
        { name: 'actions', label: '', field: 'actions', align: 'center' }
      ],
      processedStock: [],
      rawStock: stock,
      availableFilter: 'all',
      availableOptions: [
        {
          label: 'Show All',
          value: 'all'
        },
        {
          label: 'Available Only',
          value: 'available'
        },
        {
          label: 'Unavailable Only',
          value: 'unavailable'
        }
      ],
      typeFilter: 'Show All',
      typeOptions: ['Show All', 'Specimen', 'Carving', 'Cluster'],
      pagination: {
        descending: false,
        rowsPerPage: 20
      },
      flags: {
        showProductView: false
      },
      selectedProduct: null
    }
  },
  mounted () {
    this.processStockData()
  },
  computed: {
    filteredStock () {
      if (this.availableFilter === 'available') {
        return this.processedStock.filter(row => row.available)
      } else if (this.availableFilter === 'unavailable') {
        return this.processedStock.filter(row => !row.available)
      } else {
        return this.processedStock
      }
    },
    filter () {
      return { availability: this.availableFilter, type: this.typeFilter }
    }
  },
  methods: {
    formatColour (colours) {
      return colours.join(', ')
    },
    formatPrice (price) {
      return '£' + (price / 100).toFixed(2)
    },
    processStockData () {
      const result = []

      for (const s in this.rawStock) {
        result.push({
          id: s,
          ...this.rawStock[s],
          actions: ''
        })
      }

      this.processedStock = result
    },
    filterMethod () {
      let result = this.processedStock

      if (this.filter.availability === 'available') {
        result = this.processedStock.filter(row => row.available)
      } else if (this.filter.availability === 'unavailable') {
        result = this.processedStock.filter(row => !row.available)
      }

      if (this.filter.type !== 'Show All') {
        result = result.filter(row => row.type === this.filter.type)
      }

      return result
    },
    toggleAvailable (row) {
      row.available = !row.available
    },
    showAddNew () {
      this.flags.showProductView = true
    },
    showEditProduct (row) {
      this.selectedProduct = row
      this.flags.showProductView = true
    },
    clearProductView () {
      this.flags.showProductView = false
      this.selectedProduct = null
    },
    updateProduct (product) {
      const index = this.processedStock.findIndex(x => x.id === product.id)
      if (index >= 0) {
        this.processedStock.splice(index, 1, product)
      }
      this.clearProductView()
    },
    addNewProduct (product) {
      this.processedStock.push({ id: this.processStockData.length + 1, ...product })
      this.clearProductView()
    }
  }
}
</script>

<style lang="stylus">

.q-table thead
  background-color var(--q-color-secondary)

.q-table .unavailable
  background rgba(193,193,193,0.2)

</style>
