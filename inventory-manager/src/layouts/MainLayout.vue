<template lang="pug">
  q-layout(view="hHh Lpr fFf")
    q-header(elevated)
      q-toolbar
        q-btn(
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen")

        q-toolbar-title Inventory Manager

    q-drawer(
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-1"
      :width="150")

      q-list.menu-list
        q-item(
          v-for="menuItem in menuItems"
          :key="menuItem.label"
          exact
          :to="menuItem.link"
        )
          q-item-section
            q-item-label {{ menuItem.label }}

    q-page-container
      router-view

</template>

<script>

export default {
  name: 'MainLayout',
  components: { },
  data () {
    return {
      leftDrawerOpen: false,
      menuItems: [
        {
          link: '/view',
          label: 'View All'
        },
        {
          link: '/addproduct',
          label: 'Add Product'
        },
        {
          link: '/sale',
          label: 'Record Sale'
        }
      ]
    }
  }
}
</script>
