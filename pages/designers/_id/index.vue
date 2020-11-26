<template>
  <b-container>
    <h4>Designer Details:</h4>
    <p>Name: {{ designer.name }}</p>
    <p>Email: {{ designer.email }}</p>
    <h4>Projects :</h4>
    <b-table
      v-if="projects.length"
      striped
      hover
      :items="projects"
      :fields="projectsFields"
    >
      <template>
        asd
      </template>
    </b-table>
    <p v-else>
      No Porjects.
    </p>
    <nuxt-link to="/designers">
      Go Back
    </nuxt-link>
  </b-container>
</template>
<script>
export default {
  data () {
    return {
      designer: {},
      projectsFields: ['name']
    }
  },
  computed: {
    id () {
      return this.$route.params.id
    },
    projects () {
      return this.designer.projects || []
    }
  },
  created () {
    this.$axios.$get(`/api/designers/${this.id}`)
      .then((designer) => {
        this.designer = designer || {}
      })
  }
}
</script>

<style scoped>

</style>
