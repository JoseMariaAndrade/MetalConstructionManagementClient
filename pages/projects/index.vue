<template>
  <div>
    <b-container>
      <b-table striped over :items="projects" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/clients/${row.item.name}`">
            Details
          </nuxt-link>
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
      fields: ['name', 'client', 'designer', 'actions'],
      projects: []
    }
  },
  created () {
    this.$axios.$get('/api/projects')
      .then((projects) => {
        this.projects = projects
      })
  }
}
</script>

<style scoped>

</style>
