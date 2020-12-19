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
    <nuxt-link class="btn btn-primary" :to="`/manufacturers/${id}/product/${product.name}/edit`">
      Edit Product
    </nuxt-link>
    <h4>Structures:</h4>
    <b-col v-show="variants.length" lg="6" class="my-1">
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
    <br>
    <h4>Product Variants</h4>
    <b-table
      v-if="variants.length"
      :filter="filter"
      striped
      hover
      head-variant="dark"
      sticky-header="800px"
      responsive="md"
      :items="variants"
      :fields="varianteFields"
    >
      <template v-slot:cell(actions)="row">
        <button type="button" class="btn btn-danger" @click.prevent="deleteVariant(row.item.codigo)">
          Delete
        </button>
      </template>
    </b-table>
    <p v-else>
      No Variantes.
    </p>
    <b-button variant="primary" @click="toggleAddProductVariant">
      Add Product Variant
    </b-button>
    <div v-if="showAddProductVariant">
      <br>
      <h6>New Product</h6>
      <b-form style="display:inline-block" @submit.prevent="createVariant">
        <b-form-group id="variantcode-input-group" label-for="variantcode-input" label="Variant code:">
          <b-form-input
            id="variantcode-input"
            v-model.trim="formData.codigo"
            type="text"
            required
            trim
            placeholder="Enter the variant code"
          />
        </b-form-group>
        <b-form-group id="variantname-input-group" label-for="variantname-input" label="Variant name:">
          <b-form-input
            id="variantname-input"
            v-model.trim="formData.nome"
            type="text"
            required
            trim
            placeholder="Enter the variant name"
          />
        </b-form-group>
        <b-form-group id="weff_p-input-group" label-for="weff_p-input" label="weff_p:">
          <b-form-input
            id="weff_p-input"
            v-model.trim="formData.weff_p"
            type="number"
            required
            trim
            placeholder="Enter the weff_p value"
          />
        </b-form-group>
        <b-form-group id="weff_n-input-group" label-for="weff_n-input" label="weff_n:">
          <b-form-input
            id="weff_n-input"
            v-model.trim="formData.weff_n"
            type="number"
            required
            trim
            placeholder="Enter the weff_n value"
          />
        </b-form-group>
        <b-form-group id="ar-input-group" label-for="ar-input" label="ar:">
          <b-form-input
            id="ar-input"
            v-model.trim="formData.ar"
            type="number"
            required
            trim
            placeholder="Enter the ar value"
          />
        </b-form-group>
        <b-form-group id="sigmaC-input-group" label-for="sigmaC-input" label="sigmaC:">
          <b-form-input
            id="sigmaC-input"
            v-model.trim="formData.sigmaC"
            type="number"
            required
            trim
            placeholder="Enter the sigmaC value"
          />
        </b-form-group>
      </b-form>
      <br>
      <b-button variant="success" @click.prevent="createVariant">
        CREATE
      </b-button>
    </div>
    <br>
    <nuxt-link :to="`/manufacturers/${id}`">
      Go Back
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
      formData: {
        codigo: null,
        produto: null,
        nome: null,
        weff_p: null,
        weff_n: null,
        ar: null,
        sigmaC: null
      },
      product: {},
      filter: null,
      showAddProductVariant: false,
      varianteFields: ['codigo', 'nome', 'weff_p', 'weff_n', 'ar', 'sigmaC', 'actions']
    }
  },
  computed: {
    id () {
      return this.$route.params.id
    },
    name () {
      return this.$route.params.name
    },
    variants () {
      return this.product.variants || []
    }
  },
  created () {
    this.$axios.$get(`/api/manufacturers/${this.id}/product/${this.name}/variants`)
      .then((product) => {
        this.product = product || {}
      })
  },
  methods: {
    deleteVariant (codigo) {
      this.$axios.$delete(`/api/variants/${codigo}`)
        .then(() => this.$axios.$get(`/api/manufacturers/${this.id}/product/${this.name}/variants`))
        .then((product) => {
          this.product = product
        })
    },
    toggleAddProductVariant () {
      this.showAddProductVariant = !this.showAddProductVariant
    },
    createVariant () {
      this.$axios.$post('/api/variants/',
        {
          codigo: this.formData.codigo,
          produto: this.name,
          nome: this.formData.nome,
          weff_p: this.formData.weff_p,
          weff_n: this.formData.weff_n,
          ar: this.formData.ar,
          sigmaC: this.formData.sigmaC
        })
        .then(() => {
          this.$toast.success('Variante criada com sucesso').goAway(2000)
          this.$axios.$get(`/api/manufacturers/${this.id}/product/${this.name}/variants`)
            .then((product) => {
              this.product = product || {}
            })
        })
        .catch((error) => {
          this.$toast.error(error.response.data).goAway(2000)
        })
    }
  }
}
</script>

<style scoped>

</style>
