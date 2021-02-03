<template lang="pug">

  q-card(style="min-width: 30vw;")
    q-toolbar.text-white.q-pa-xs.q-pl-md.bg-ac-purple
      q-toolbar-title {{ title }}
      q-btn(flat round dense icon="close" v-close-popup @click="close")
    q-card-section.q-pa-sm.q-px-md
      q-form.q-ma-xs
        q-input.q-ma-sm(
          v-model="productData.id"
          label="ID Number"
          readonly
        )
        q-select.q-ma-sm(
          v-model="productData.type"
          label="Type"
          :options="typeOptions"
          stack-label
        )
        q-input.q-ma-sm(
          v-model="productData.material"
          label="Material"
          clearable
          clear--icon="close"
          stack-label
        )
        q-input.q-ma-sm(
          v-model="productData.shape"
          label="Shape"
          clearable
          clear--icon="close"
          stack-label
        )
        q-select.q-ma-sm(
          v-model="productData.colour"
          label="Colour"
          :options="colourOptions"
          multiple
          stack-label
        )
        q-input.q-ma-sm(
          v-model="productData.weight"
          label="Weight (g)"
          stack-label
        )
        q-input.q-ma-sm(
          v-model="productData.price"
          label="Price (£)"
          stack-label
          mask="#.##"
          fill-mask="0"
          reverse-fill-mask
        )
        q-checkbox.q-ma-sm(
          v-model="productData.available"
          label="Available: "
          left-label
        )
      div
        q-btn.float-right.q-my-md(
          label="Save"
          @click="save"
          color="primary"
        )

</template>

<script>
export default {
  name: 'ViewAll',
  props: {
    title: {
      type: String,
      required: true,
      default: ''
    },
    product: {
      type: Object,
      required: false,
      default: () => ({})
    }
  },
  data () {
    return {
      productData: { ...this.product },
      // !!! options should be from type interface/json also available to functions or straight from firestore
      typeOptions: ['Carving', 'Specimen', 'Cluster'],
      colourOptions: ['Blue', 'Green', 'Purple', 'Mixed']
    }
  },
  methods: {
    save () {
      let price = this.productData.price
      if (typeof price === 'string') {
        price = parseInt(Math.round(parseFloat(price) * 100))
      }

      this.$emit('save', { ...this.productData, price })
    },
    close () {
      this.$emit('close')
    }
  }
}
</script>

<style>

</style>
