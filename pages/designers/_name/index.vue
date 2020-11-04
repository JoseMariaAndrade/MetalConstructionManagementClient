<template>
  <b-container>
    <h4>Designer Details:</h4>
    <p>Name: {{ designer.name }}</p>
    <p>Email: {{ designer.email }}</p>
    <h4>Projects :</h4>
    <nuxt-link to="/designers">
      Go Back
    </nuxt-link>
  </b-container>
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
  }
}
</script>

<style scoped>

</style>
