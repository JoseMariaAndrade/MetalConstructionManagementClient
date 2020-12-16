<template>
  <b-container>
    <h4>Product Details:</h4>
    <p>Name: {{ product.name }}</p>
    <p>Type: {{ product.type }}</p>
    <p>Family: {{ product.family }}</p>
    <p>
      Need Stock:
      <b-icon-check-circle-fill v-if="product.needStock" variant="success" />
      <b-icon-x-circle-fill v-else variant="danger" />
    </p>
    <nuxt-link :to="`/manufacturers/${id}`">
      Go Back
    </nuxt-link>
    <nuxt-link :to="`/manufacturers/${id}/product/${product.name}/edit`">
      Edit
    </nuxt-link>
  </b-container>
</template>
<script>
import { BIconCheckCircleFill, BIconXCircleFill } from 'bootstrap-vue'
export default {
  components: {
    BIconCheckCircleFill,
    BIconXCircleFill
  },
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
