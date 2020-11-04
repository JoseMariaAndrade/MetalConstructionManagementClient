<template>
  <div>
    <b-form @submit.prevent="edit">
      <b-form-group id="name-input-group" label-for="name-input" label="Name:">
        <b-form-input id="name-input" v-model="designer.name" type="text" />
      </b-form-group>
      <b-form-group id="email-input-group" label-for="email-input" label="Email:">
        <b-form-input id="email-input" v-model="designer.email" type="email" />
      </b-form-group>
    </b-form>
    <nuxt-link to="/designers">
      Return
    </nuxt-link>
    <b-button variant="warning" type="reset">
      RESET
    </b-button>
    <b-button variant="success" @click.prevent="edit">
      CREATE
    </b-button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      designer: {}
    }
  },
  computed: {
    name () {
      return this.$route.params.name
    },
    projects () {
      return this.designer.projects || []
    }
  },
  created () {
    this.$axios.$get(`/api/designers/${this.name}`)
      .then((designer) => {
        this.designer = designer || {}
      })
  },
  methods: {
    edit () {
      this.$axios.$post('/api/designers', {
        name: this.designer.name,
        email: this.designer.email
      }).then(() => {
        this.$router.push('/designers')
      })
    }
  }
}
</script>

<style scoped>

</style>
