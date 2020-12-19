<template>
  <b-container>
    <h4>Designer Details:</h4>
    <p>Name: {{ designer.name }}</p>
    <p>Email: {{ designer.email }}</p>
    <nuxt-link :to="`/designers/${id}/project/create`">
      Create New Project
    </nuxt-link>
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
        <nuxt-link class="btn btn-link" :to="`/designers/${id}/project/${row.item.name}`">
          Details
        </nuxt-link>
        <button type="button" class="btn btn-danger" @click.prevent="deleteProject(row.item.name)">
          Delete
        </button>
        <b-button v-show="!row.item.availableToClient" @click.prevent="makeAvailable(row.item.name)">
          Available To Client
        </b-button>
      </template>
    </b-table>
    <p v-else>
      No Projects.
    </p>
  </b-container>
</template>
<script>
export default {
  data () {
    return {
      designer: {},
      projectsFields: ['name', 'actions'],
      filter: null
    }
  },
  computed: {
    id () {
      return this.$auth.user.subID
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
  },
  methods: {
    deleteProject (projectName) {
      this.$axios.$delete(`/api/projects/${projectName}`)
        .then(() => this.$axios.$get('/api/projects/'))
        .then((projects) => {
          this.projects = projects
        })
    },
    makeAvailable (projectName) {
      this.$axios.$get(`/api/designers/${this.id}/project/${projectName}/availableToClient`).then(() =>
        this.$axios.$get(`/api/designers/${this.id}`)
          .then((designer) => {
            this.designer = designer || {}
          })
      )
    }
  }
}
</script>

<style scoped>

</style>
