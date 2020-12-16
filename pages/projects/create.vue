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

      <b-form-group id="client-input-group" label-for="client-select" label="Client:">
        <b-form-select
          id="client-select"
          v-model.trim="formData.client"
          type="text"
          required
          value-field="name"
        >
          <template v-slot:first>
            <option :value="null" disabled>
              --- Please select the Client ---
            </option>
          </template>
          <template v-for="client in clients">
            <option :key="client.name" :value="client.name">
              {{ client.name }}
            </option>
          </template>
        </b-form-select>
      </b-form-group>

      <b-form-group id="designer-input-group" label-for="designer-select" label="Designer:">
        <b-form-select
          id="designer-select"
          v-model.trim="formData.designer"
          type="text"
          required
          value-field="name"
        >
          <template v-slot:first>
            <option :value="null" disabled>
              --- Please select the Designer ---
            </option>
          </template>
          <template v-for="designer in designers">
            <option :key="designer.name" :value="designer.name">
              {{ designer.name }}
            </option>
          </template>
        </b-form-select>
      </b-form-group>
    </b-form>
    <nuxt-link to="/projects">
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
        client: null,
        designer: null
      },
      clients: [],
      designers: [],
      errorMessage: false
    }
  },
  created () {
    this.$axios.$get('api/clients')
      .then((clients) => {
        this.clients = clients || []
      })
      .then(() => this.$axios.$get('/api/designers/'))
      .then((designers) => {
        this.designers = designers || []
      })
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
      this.$axios.$post('/api/projects',
        {
          name: this.formData.name,
          client: this.formData.client,
          designer: this.formData.designer
        })
        .then(() => {
          this.$router.push('/projects')
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
