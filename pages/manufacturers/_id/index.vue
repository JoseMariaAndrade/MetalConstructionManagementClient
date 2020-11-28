<template>
  <b-container class="bv-example-row">
    <h4>Manufacturer Details:</h4>
    <p>Name: {{ manufacturer.name }}</p>
    <p>Email: {{ manufacturer.email }}</p>
    <h4>Products :</h4>
    <b-col v-show="products.length" lg="6" class="my-1">
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
      v-if="products.length"
      :filter="filter"
      striped
      hover
      :items="products"
      :fields="productsFields"
    >
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" :to="`/manufacturers/${id}/product/${row.item.name}`">
          Details
        </nuxt-link>
        <b-button class="btn btn-danger" @click="deleteProduct(row.item)">
          Delete
        </b-button>
      </template>
    </b-table>
    <p v-else>
      No products.
    </p>
    <nuxt-link :to="`/manufacturers/${id}/product/create`">
      Create New Product
    </nuxt-link>
    <nuxt-link to="/manufacturers">
      Go Back
    </nuxt-link>
  </b-container>
</template>
<script>
export default {
  data () {
    return {
      manufacturer: {},
      productsFields: [
        {
          key: 'name',
          sortable: true
        },
        {
          key: 'type',
          sortable: true
        },
        {
          key: 'family',
          sortable: true
        }, 'actions'],
      filter: null
    }
  },
  computed: {
    id () {
      return this.$route.params.id
    },
    products () {
      return this.manufacturer.products || []
    }
  },
  created () {
    this.$axios.$get(`/api/manufacturers/${this.id}`)
      .then((manufacturer) => {
        this.manufacturer = manufacturer || {}
      })
  },
  methods: {
    deleteProduct (product) {
      this.$axios.$delete(`/api/manufacturers/${this.id}/product/${product.name}`)
      // this.$delete(this.products, product)
      this.products.splice(this.products.indexOf(product), 1)
    }
  }
}
</script>

<style scoped>

</style>
