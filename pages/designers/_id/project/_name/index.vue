<template>
  <b-container>
    <h4>Project Details:</h4>
    <p>Name: {{ project.name }}</p>
    <p>Client: {{ project.nameClient }}</p>
    <p>Designer: {{ project.nameDesigner}}</p>
    <nuxt-link :to="`/designers/${id}/project/${projectName}/structure/create`">
      Create New Structure
    </nuxt-link>
    <h4>Structures :</h4>
    <b-col v-show="structures.length" lg="6" class="my-1">
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
      v-if="structures.length"
      :filter="filter"
      striped
      hover
      :items="structures"
      :fields="fields"
    >
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-info" :to="`/designers/${id}/project/${projectName}/structure/${row.item.name}`">
          Details
        </nuxt-link>
        <b-button class="btn btn-danger" @click="deleteStructure(row.item.name)">
          Delete
        </b-button>
      </template>
    </b-table>
    <p v-else>
      No Structures.
    </p>
    <nuxt-link :to="`/designers/${id}`">
      Go Back
    </nuxt-link>
  </b-container>
</template>

<script>
export default {
  auth: true,
  data () {
    return {
      project: {},
      designers: [],
      designer: null,
      showChangeDesigner: false,
      fields: ['name', 'actions'],
      filter: null
    }
  },
  computed: {
    id () {
      return this.$auth.user.subID
    },
    projectName () {
      return this.$route.params.name
    },
    structures () {
      return this.project.structures || []
    }
  },
  created () {
    this.$axios.$get(`/api/projects/${this.projectName}`)
      .then((project) => {
        this.project = project || {}
      })
  },
  methods: {
    editDesigner () {
      this.$axios.$put(`api/projects/${this.project.name}/designers/${this.designer}`)
        .then(() => this.$axios.$get(`/api/projects/${this.name}`))
        .then((project) => {
          this.project = project || {}
        })
    },
    deleteStructure (structureName) {
      this.$axios.$delete(`/api/projects/${this.project.name}/structures/${structureName}`)
        .then(() => this.$axios.$get(`/api/projects/${this.name}`))
        .then((project) => {
          this.project = project || {}
        })
    }
  }
}
</script>

<style scoped>

</style>
