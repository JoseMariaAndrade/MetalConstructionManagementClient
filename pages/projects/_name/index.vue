<template>
  <b-container>
    <h4>Project Details:</h4>
    <p>Name: {{ project.name }}</p>
    <p>Client Name: {{ project.client.name }}</p>
    <p>Designer Name: {{ project.designer.name }}</p>

    <nuxt-link to="/projects">
      Go Back
    </nuxt-link>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      project: {}
    }
  },
  computed: {
    name () {
      return this.$route.params.name
    }
  },
  created () {
    this.$axios.$get(`/api/projects/${this.name}`)
      .then((project) => {
        this.project = project || {}
      })
  }
}
</script>

<style scoped>

</style>
