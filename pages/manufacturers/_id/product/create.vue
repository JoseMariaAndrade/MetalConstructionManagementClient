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
          v-model.trim="formData.name"
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
          v-model="formData.typeDescription"
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
          v-model="formData.familyName"
          :state="isFamilyValid"
          required
          value-field="code"
        >
          <template v-slot:first>
            <option :value="null" disabled>
              --- Please select the Family ---
            </option>
          </template>
          <template v-for="family in families">
            <option :key="family.name" :value="family.name">
              {{ family.name }}
            </option>
          </template>
        </b-form-select>
      </b-form-group>
      <div>
        <b-form-group
          id="altura-input-group"
          label-for="altura-input"
          label="altura:"
          label-cols-sm="1"
          label-cols-lg="1"
          description="mm"
          :invalid-feedback="invalidContactFeedback"
        >
          <b-form-input
            id="altura-input"
            v-model.trim="formData.altura"
            :state="isContactValid"
            type="number"
            required
          />
        </b-form-group>
      </div>
      <div>
        <b-form-group
          id="espessura-input-group"
          label-for="espessura-input"
          label="espessura:"
          label-cols-sm="1"
          label-cols-lg="1"
          description="mm"
          :invalid-feedback="invalidContactFeedback"
        >
          <b-form-input
            id="espessura-input"
            v-model.trim="formData.espessura"
            :state="isContactValid"
            type="number"
            required
          />
        </b-form-group>
      </div>
      <div>
        <b-form-group
          id="peso-input-group"
          label-for="peso-input"
          label="peso:"
          label-cols-sm="1"
          label-cols-lg="1"
          description="kg/m"
          :invalid-feedback="invalidContactFeedback"
        >
          <b-form-input
            id="peso-input"
            v-model.trim="formData.peso"
            :state="isContactValid"
            type="number"
            required
          />
        </b-form-group>
      </div>
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
        name: null,
        typeDescription: null,
        familyName: null
      },
      families: [],
      types: []
    }
  },
  computed: {
    id () {
      return this.$route.params.id
    }
  },
  created () {
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