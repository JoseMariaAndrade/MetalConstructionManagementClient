<template>
  <b-container>
    <h4>Client Details:</h4>
    <p>Name: {{ client.name }}</p>
    <p>Email: {{ client.email }}</p>
    <p>Contact: {{ client.contact }}</p>
    <p>Address: {{ client.address }}</p>
    <h4>Projects :</h4>
    <b-table
      v-if="projects.length"
      striped
      hover
      :items="projects"
      :fields="projectsFields"
    >
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" :to="`/clients/${id}/project/${row.item.name}`">
          Details
        </nuxt-link>
      </template>
    </b-table>
    <p v-else>
      No Projects.
    </p>
    <nuxt-link to="/clients">
      Go Back
    </nuxt-link>
  </b-container>
</template>
<script>
export default {
  data () {
    return {
      client: {},
      projectsFields: ['name', 'nameDesigner', 'decision', 'actions']
    }
  },
  computed: {
    id () {
      return this.$route.params.id
    },
    projects () {
      return this.client.projects || []
    }
  },
  created () {
    this.$axios.$get(`/api/clients/${this.id}`)
      .then((client) => {
        this.client = client || {}
      })
  },
  methods: {
  }
}
</script>

<style scoped>

</style>
