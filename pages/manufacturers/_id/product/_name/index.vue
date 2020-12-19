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
    <h4>Structures:</h4>
    <b-col v-show="variantes.length" lg="6" class="my-1">
      <b-form-group
        label="Filter"
        label-cols-sm="1"
        label-align-sm="right"
        label-size="sm"
        label-for="filterInput"
        class="mb-0"
      >
        <b-input-group size="sm">
          <b-form-input
            id="filterInput"
            v-model="filter"
            type="search"
            placeholder="Type to Search"
          />
          <b-input-group-append>
            <b-button :disabled="!filter" @click="filter = ''">
              Clear
            </b-button>
          </b-input-group-append>
        </b-input-group>
      </b-form-group>
    </b-col>
    <b-table
      v-if="variantes.length"
      :filter="filter"
      striped
      hover
      head-variant="dark"
      sticky-header="800px"
      responsive="md"
      :items="variantes"
      :fields="variantesFields"
    >
    </b-table>
    <p v-else>
      No Variantes.
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
      product: {},
      filter: null
    }
  },
  computed: {
    id () {
      return this.$route.params.id
    },
    name () {
      return this.$route.params.name
    },
    variantes () {
      return this.product.variantes || []
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
