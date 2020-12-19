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
          v-model="formData.type"
          :state="isTypeValid"
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
          v-model="formData.family"
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
        type: null,
        family: null
      },
      families: [],
      types: []
    }
  },
  computed: {
    id () {
      return this.$auth.user.subID
    },
    invalidNameFeedback () {
      if (!this.formData.name) {
        return null
      }
      const usernameLen = this.formData.name.length
      if (usernameLen < 3 || usernameLen > 30) {
        return 'The name must be between [3,30] characters.'
      }
      return ''
    },
    isNameValid () {
      if (this.invalidNameFeedback === null) {
        return null
      }

      return this.invalidNameFeedback === ''
    },
    isTypeValid () {
      if (!this.formData.type) {
        return null
      }
      return this.types.some(type => this.formData.type === type.description)
    },
    isFamilyValid () {
      if (!this.formData.family) {
        return null
      }
      return this.families.some(family => this.formData.family === family.name)
    },
    isFormValid () {
      if (!this.isNameValid) {
        return false
      }
      if (!this.isTypeValid) {
        return false
      }
      if (!this.isFamilyValid) {
        return false
      }
      return true
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
          this.formData.family = null
        })
    },
    reset () {
      this.errorMessage = false
    },
    create () {
      this.$axios.$post(`/api/manufacturers/${this.id}/product/create`, this.formData)
        .then(() => {
          this.$router.push(`/manufacturers/${this.id}`)
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
