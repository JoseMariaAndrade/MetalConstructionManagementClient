<template>
  <b-container>
    <b-table :items="projects" :fields="fields">
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-info" :to="`/clients/${clientName}/projects/${row.item.name}`">
          Project Details
        </nuxt-link>
      </template>
    </b-table>
    <nuxt-link :to="/clients/">
      Go Back
    </nuxt-link>
  </b-container>
</template>
<script>
export default {
  data () {
    return {
      projects: [],
      fields: ['name', 'client', 'designer', 'actions']
    }
  },
  computed: {
    clientName () {
      return this.$route.params.name
    }
  },
  created () {
    this.$axios.$get(`/api/clients/${this.clientName}/projects`)
      .then((projects) => {
        this.projects = projects
      })
  }
}
</script>
<style>

</style>
