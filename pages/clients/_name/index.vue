<template>
  <b-container>
    <h4>Client Details:</h4>
    <p>Name: {{ client.name }}</p>
    <p>Email: {{ client.email }}</p>
    <p>Contact: {{ client.contact }}</p>
    <p>Address: {{ client.address }}</p>
    <h4>Projects :</h4>
    <nuxt-link to="/clients">
      Go Back
    </nuxt-link>
  </b-container>
</template>
<script>
export default {
  data () {
    return {
      client: {}
    }
  },
  computed: {
    name () {
      return this.$route.params.name
    },
    projects () {
      return this.client.projects || []
    }
  },
  created () {
    this.$axios.$get(`/api/clients/${this.name}`)
      .then((client) => {
        this.client = client || {}
      })
  }
}
</script>

<style scoped>

</style>
