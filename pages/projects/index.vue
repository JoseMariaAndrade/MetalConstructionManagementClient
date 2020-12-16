<template>
  <div>
    <b-container>
      <b-table striped over :items="projects" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/projects/${row.item.name}`">
            Details
          </nuxt-link>
          <button type="button" class="btn btn-danger" @click.prevent="deleteProject(row.item.name)">
            Delete
          </button>
        </template>
</b-table>
    </b-container>
    <nuxt-link to="/projects/create">
      Create a New Project
    </nuxt-link>
  </div>
</template>

<script>
export default {
  data () {
    return {
      fields: ['name', 'nameClient', 'nameDesigner', 'actions'],
      projects: []
    }
  },
  created () {
    this.$axios.$get('/api/projects')
      .then((projects) => {
        this.projects = projects
      })
  },
  methods: {
    deleteProject (projectName) {
      this.$axios.$delete(`/api/projects/${projectName}`)
        .then(() => this.$axios.$get('/api/projects/'))
        .then((projects) => {
          this.projects = projects
        })
    }
  }
}
</script>

<style scoped>

</style>
