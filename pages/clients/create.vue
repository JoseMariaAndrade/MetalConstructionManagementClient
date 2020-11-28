<template>
  <div>
    <b-form :disabled="!isFormValid" @submit.prevent="create">
      <b-form-group id="name-input-group" label-for="name-input" :state="isNameValid" :invalid-feedback="invalidNameFeedback" label="Name:">
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
      <b-form-group id="email-input-group" label-for="email-input" label="Email:">
        <b-form-input
          id="email-input"
          v-model.trim="formData.email"
          type="email"
          required
          :state="isEmailValid"
        />
      </b-form-group>
      <b-form-group id="password-input-group" label-for="password-input" label="Password">
        <b-form-input
          id="password-input"
          v-model="formData.password"
          :state="isPasswordValid"
          required
          type="password"
        />
      </b-form-group>
      <b-form-group id="contact-input-group" label-for="contact-input" label="Contact:" :invalid-feedback="invalidContactFeedback">
        <b-form-input
          id="contact-input"
          v-model.trim="formData.contact"
          :state="isContactValid"
          type="number"
          required
        />
      </b-form-group>
      <b-form-group id="address-input-group" label-for="address-input" label="Address:" :invalid-feedback="invalidAddressFeedback">
        <b-form-input
          id="address-input"
          v-model.trim="formData.address"
          :state="isAddressValid"
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
    <b-button variant="success" :disabled="!isFormValid" @click.prevent="create">
      CREATE
    </b-button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      formData: {
        name: null,
        password: null,
        email: null,
        contact: null,
        address: null
      },
      errorMessage: false
    }
  },
  computed: {
    invalidNameFeedback () {
      if (!this.formData.name) {
        return null
      }
      const usernameLen = this.formData.name.length
      if (usernameLen < 3 || usernameLen > 15) {
        return 'The username must be between [3,15] characters.'
      }
      return ''
    },
    isNameValid () {
      if (this.invalidNameFeedback === null) {
        return null
      }

      return this.invalidNameFeedback === ''
    },
    isPasswordValid () {
      if (!this.formData.password) {
        return null
      }

      const passwordLen = this.formData.password.length
      if (passwordLen < 3 || passwordLen > 255) {
        return false
      }
      return true
    },
    isEmailValid () {
      if (!this.formData.email) {
        return null
      }

      return true
    },
    invalidContactFeedback () {
      if (!this.formData.contact) {
        return null
      }
      const contactLength = this.formData.contact.length
      if (contactLength < 9) {
        return 'The contact must have at lest 9 characters.'
      }
      return ''
    },
    isContactValid () {
      if (this.invalidContactFeedback === null) {
        return null
      }

      return this.invalidContactFeedback === ''
    },
    invalidAddressFeedback () {
      if (!this.formData.address) {
        return null
      }
      const addressLength = this.formData.address.length
      if (addressLength < 3 || addressLength > 15) {
        return 'The address must be between [3,15] characters.'
      }
      return ''
    },
    isAddressValid () {
      if (this.invalidAddressFeedback === null) {
        return null
      }

      return this.invalidAddressFeedback === ''
    },
    isFormValid () {
      if (!this.isNameValid) {
        return false
      }
      if (!this.isPasswordValid) {
        return false
      }
      if (!this.isEmailValid) {
        return false
      }
      if (!this.isContactValid) {
        return false
      }
      if (!this.isAddressValid) {
        return false
      }
      return true
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
