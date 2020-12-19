<template>
  <b-container>
    <b-form @submit.prevent="create">
      <b-form-group id="name-input-group" label-for="name-input" label="Name:">
        <b-form-input
          id="name-input"
          v-model.trim="formData.name"
          type="text"
          required
          trim
          placeholder="Enter a structure name"
        />
      </b-form-group>
      <b-form-group id="nb-input-group" label-for="name-input" label="NB:">
        <b-form-input
          id="name-input"
          v-model.trim="formData.nb"
          type="number"
          required
          trim
          placeholder="Enter a nb"
        />
      </b-form-group>
      <b-form-group id="LVao-input-group" label-for="name-input" label="LVao:">
        <b-form-input
          id="name-input"
          v-model.trim="formData.LVao"
          type="number"
          required
          trim
          placeholder="Enter a LVao"
        />
      </b-form-group>
      <b-form-group id="q-input-group" label-for="name-input" label="q:">
        <b-form-input
          id="name-input"
          v-model.trim="formData.q"
          type="number"
          required
          trim
          placeholder="Enter a q"
        />
      </b-form-group>
      <b-form-group id="client-input-group" label-for="client-select" label="Product:">
        <b-form-select
          id="product-select"
          v-model.trim="product"
          type="text"
          required
          value-field="name"
        >
          <template v-slot:first>
            <option :value="null" disabled>
              --- Please select the Product ---
            </option>
          </template>
          <template v-for="product in products">
            <option :key="product.name" :value="product.name">
              {{ product.name }}
            </option>
          </template>
        </b-form-select>
      </b-form-group>
      <b-button @click.prevent="addProduct">
        Add Product
      </b-button>
      <hr>
      <b-table
        v-if="formData.products.length"
        :filter="filter"
        striped
        hover
        head-variant="dark"
        sticky-header="400px"
        responsive="md"
        :items="formData.products"
        :fields="productFields"
      >
        <template v-slot:cell(actions)="row">
          <b-button variant="danger" size="sm" @click.prevent="deleteProduct(row.item.name)">
            Delete
          </b-button>
        </template>
      </b-table>
    </b-form>
    <nuxt-link :to="`/designers/${id}/project/${name}`">
      Return
    </nuxt-link>
    <b-button variant="warning" type="reset" @click="reset">
      RESET
    </b-button>
    <b-button variant="success" @click.prevent="create">
      CREATE
    </b-button>

    <nuxt-link to="/simulation">
      Simulation
    </nuxt-link>
  </b-container>
</template>

<script>
export default {
  auth: true,
  data () {
    return {
      productFields: ['name', 'actions'],
      formData: {
        name: null,
        nb: null,
        LVao: null,
        q: null,
        products: [],
        project: null
      },
      products: [],
      product: null,
      errorMessage: false
    }
  },
  computed: {
    id () {
      return this.$auth.user.subID
    },
    name () {
      return this.$route.params.name
    }
  },
  created () {
    this.$axios.$get('/api/products')
      .then((products) => {
        this.products = products
      })
  },
  methods: {
    reset () {
      this.errorMessage = false
    },
    create () {
      console.log(this.formData)
      this.formData.project = this.name
      this.$axios.$post('/api/structures/', this.formData)
        .then(() => {
          this.$router.push(`/designers/${this.id}`)
        })
        .catch((error) => {
          this.errorMessage = error.response.data
          this.$toast.error('Error').goAway(2000)
        })
    },
    addProduct () {
      this.formData.products.push({ name: this.product })
    },
    deleteProduct (product) {
      this.formData.products.splice(this.formData.products.indexOf(product), 1)
    }
  }
}
</script>

<style scoped>
</style>
