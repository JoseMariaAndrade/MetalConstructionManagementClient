<template>
  <div>
    <div>
      <div class="form-group" :class="{ 'form-group--error': $v.formData.name.$error }">
        <label class="form__label">Nested A</label>
        <input v-model.trim="$v.formData.name.$model" class="form__input">
      </div>
      <div v-if="!$v.formData.name.required" class="error">
        Field is required.
      </div>
    </div>
    <b-form @submit.prevent="create">
      <b-form-group id="name-input-group" label-for="name-input" label="Name:">
        <b-form-input
          id="name-input"
          v-model.trim="formData.name"
          type="text"
          required
          trim
          placeholder="Enter a name"
        />
      </b-form-group>
      <b-form-group id="email-input-group" label-for="email-input" label="Email:">
        <b-form-input
          id="email-input"
          v-model.trim="formData.email"
          type="email"
          required
        />
      </b-form-group>
      <b-form-group id="contact-input-group" label-for="contact-input" label="Contact:">
        <b-form-input
          id="contact-input"
          v-model.trim="formData.contact"
          type="number"
          required
        />
      </b-form-group>
      <b-form-group id="address-input-group" label-for="address-input" label="Address:">
        <b-form-input
          id="address-input"
          v-model.trim="formData.address"
          type="text"
          required
        />
      </b-form-group>
    </b-form>
    <nuxt-link to="/clients">
      Return
    </nuxt-link>
    <b-button variant="warning" type="reset" @click="reset">
      RESET
    </b-button>
    <b-button variant="success" @click.prevent="create">
      CREATE
    </b-button>
  </div>
</template>

<script>
import Vue from 'vue'
import Vuelidate from 'vuelidate'
import { required } from 'vuelidate/lib/validators'
Vue.use(Vuelidate)

export default {
  data () {
    return {
      formData: {
        name: null,
        email: null
      },
      errorMessage: false
    }
  },
  validations: {
    formData: {
      name: {
        required
      },
      email: {
        required
      }
    }
  },
  methods: {
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
