<template>
  <b-container>
    <h4>Product Details:</h4>
    <p>Name: {{ product.name }}</p>
    <p>Type: {{ product.type }}</p>
    <p>Family: {{ product.family }}</p>
    <nuxt-link :to="`/manufacturers/${id}`">
      Go Back
    </nuxt-link>
    <nuxt-link :to="`/manufacturers/${id}/product/${product.name}/edit`">
      Edit
    </nuxt-link>
  </b-container>
</template>
<script>
export default {
  data () {
    return {
      product: {}
    }
  },
  computed: {
    id () {
      return this.$route.params.id
    },
    name () {
      return this.$route.params.name
    }
  },
  created () {
    this.$axios.$get(`/api/manufacturers/${this.id}/product/${this.name}`)
      .then((product) => {
        this.product = product || {}
      })
  }
}
</script>

<style scoped>

</style>
