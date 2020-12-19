<template>
  <b-container>
    <h4>Client Details:</h4>
    <p>Name: {{ client.name }}</p>
    <p>Email: {{ client.email }}</p>
    <p>Contact: {{ client.contact }}</p>
    <p>Address: {{ client.address }}</p>
    <h4>Projects :</h4>
    <b-col v-show="projects.length" lg="6" class="my-1">
      <b-form-group
        label="Filter"
        label-cols-sm="1"
        label-align-sm="right"
        label-size="sm"
        label-for="filterInput"
        class="mb-0"
      >
        <b-input-group size="sm">
          <b-form-input
            id="filterInput"
            v-model="filter"
            type="search"
            placeholder="Type to Search"
          />
          <b-input-group-append>
            <b-button :disabled="!filter" @click="filter = ''">
              Clear
            </b-button>
          </b-input-group-append>
        </b-input-group>
      </b-form-group>
    </b-col>
    <b-table
      v-if="projects.length"
      :filter="filter"
      striped
      hover
      :items="projects"
      :fields="projectsFields"
    >
      <template v-slot:cell(actions)="row">
        <nuxt-link v-show="row.item.availableToClient" class="btn btn-link" :to="`/clients/${id}/project/${row.item.name}`">
          Details
        </nuxt-link>
      </template>
    </b-table>
    <p v-else>
      No Projects.
    </p>
  </b-container>
</template>
<script>
export default {
  auth: true,
  data () {
    return {
      client: {},
      projectsFields: [
        {
          key: 'name',
          sortable: true
        }, { key: 'nameDesigner', sortable: true }, 'actions'],
      filter: null
    }
  },
  computed: {
    id () {
      return this.$auth.user.subID
    },
    projects () {
      return this.client.projects || []
    }
  },
  created () {
    this.$axios.$get(`/api/clients/${this.id}`)
      .then((client) => {
        this.client = client || {}
      }).catch(() => {
        this.$router.push('/')
      })
  }
}
</script>

<style scoped>

</style>
