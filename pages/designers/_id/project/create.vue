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
            <option :key="client.name" :value="client.id">
              {{ client.name }}
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
  </b-container>
</template>

<script>
export default {
  auth: true,
  data () {
    return {
      formData: {
        name: null,
        client: null,
        designer: null
      },
      clients: [],
      errorMessage: false
    }
  },
  computed: {
    id () {
      return this.$auth.user.subID
    }
  },
  created () {
    this.$axios.$get('/api/clients/')
      .then((clients) => {
        this.clients = clients || []
      })
    this.formData.designer = this.id
  },
  methods: {
    reset () {
      this.errorMessage = false
    },
    create () {
      this.$axios.$post('/api/projects',
        {
          name: this.formData.name,
          idClient: this.formData.client,
          idDesigner: this.formData.designer
        })
        .then(() => {
          this.$router.push(`/designers/${this.id}`)
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
