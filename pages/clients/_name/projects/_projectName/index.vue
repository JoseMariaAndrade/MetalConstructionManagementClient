<template>
  <b-container>
    <h4>Project Details:</h4>
    <p>Name: {{ project.name }}</p>
    <p>Client: {{ project.client }}</p>
    <p>Designer: {{ project.designer }}</p>

    <h6>Project Structures :</h6>
    <b-table striped over :items="structures" :fields="fields">
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-info" :to="`/clients/${clientName}/projects/${projectName}/structures/${row.item.name}`">
          Details
        </nuxt-link>
      </template>
    </b-table>
    <nuxt-link :to="`/clients/${project.client}/projects`">
      Go Back
    </nuxt-link>
  </b-container>
</template>
<script>
export default {
  data () {
    return {
      project: {},
      fields: ['name', 'project', 'actions']
    }
  },
  computed: {
    clientName () {
      return this.$route.params.name
    },
    projectName () {
      return this.$route.params.projectName
    },
    structures () {
      return this.project.structures
    }
  },
  created () {
    this.$axios.$get(`/api/projects/${this.projectName}`)
      .then((project) => {
        this.project = project || {}
      })
  }
}
</script>
<style scoped>

</style>
