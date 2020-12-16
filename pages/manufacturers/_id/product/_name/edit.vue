<template>
  <b-container>
    <b-form :disabled="!isFormValid" @submit.prevent="create">
      <b-form-group
        id="name-input-group"
        label-for="name-input"
        :state="isNameValid"
        :invalid-feedback="invalidNameFeedback"
        label="Name:"
        label-cols-sm="1"
        label-cols-lg="1"
      >
        <b-form-input
          id="name-input"
          v-model.trim="product.name"
          type="text"
          :state="isNameValid"
          required
          trim
          placeholder="Enter a name"
        />
      </b-form-group>
      <b-form-group
        id="type-input-group"
        label="Type:"
        label-for="type-input"
        label-cols-sm="1"
        label-cols-lg="1"
      >
        <b-form-select
          id="type-input"
          v-model="product.type"
          :state="isCourseValid"
          required
          value-field="code"
          @change="getFamilies($event)"
        >
          <template v-slot:first>
            <option :value="null" disabled>
              --- Please select the Type ---
            </option>
          </template>
          <template v-for="type in types">
            <option :key="type.description" :value="type.description">
              {{ type.description }}
            </option>
          </template>
        </b-form-select>
      </b-form-group>
      <b-form-group
        id="family-input-group"
        label="Family:"
        label-for="family-input"
        label-cols-sm="1"
        label-cols-lg="1"
      >
        <b-form-select
          id="family-input"
          v-model="product.family"
          :state="isFamilyValid"
          required
          value-field="code"
        >
          <template v-slot:first>
            <option :value="product.family">
              {{ product.family }}
            </option>
          </template>
          <template v-for="family in families">
            <option :key="family.name" :value="family.name">
              {{ family.name }}
            </option>
          </template>
        </b-form-select>
      </b-form-group>
    </b-form>
    <nuxt-link :to="`/manufacturers/${id}`">
      Return
    </nuxt-link>
    <b-button variant="warning" type="reset" @click="reset">
      RESET
    </b-button>
    <b-button variant="success" :disabled="!isFormValid" @click.prevent="create">
      CREATE
    </b-button>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      formData: {
        namew: null,
        typeDescription: null,
        familyName: null
      },
      product: {
        name: null,
        family: null,
        type: null
      },
      families: [],
      types: []
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
    this.$axios.$get('/api/types')
      .then((types) => {
        this.types = types
      })
  },
  methods: {
    getFamilies (value) {
      this.$axios.$get(`/api/types/${value}`)
        .then((families) => {
          this.families = families.families
          this.formData.familyName = null
        })
    },
    reset () {
      this.errorMessage = false
    },
    create () {
      this.$axios.$post('/api/clients', this.formData)
        .then(() => {
          this.$router.push('/clients')
        })
        .catch((error) => {
          this.errorMessage = error.response.data
        })
    }
  }
}
</script>

<style scoped>

</style>
