<template>
  <div>
    <b-container>
      <b-table striped over :items="products" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/products/${row.item.name}`">
            Details
          </nuxt-link>
        </template>
      </b-table>
    </b-container>
    <nuxt-link to="/product/create">
      Create a New Product
    </nuxt-link>
  </div>
</template>

<script>
export default {
  data () {
    return {
      fields: ['name', 'type', 'family', 'actions'],
      products: []
    }
  },
  created () {
    this.$axios.$get('/api/products')
      .then((products) => {
        this.products = products
      })
  }
}
</script>

<style scoped>

</style>
