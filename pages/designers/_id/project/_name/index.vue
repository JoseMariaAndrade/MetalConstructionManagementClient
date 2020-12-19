<template>
  <b-container>
    <h4>Project Details:</h4>
    <p>Name: {{ project.name }}</p>
    <p>Client: {{ project.client }}</p>
    <span>    <p style="display: inline-block">Designer: {{ project.designer }}</p>
      <b-button class="btn btn-info" @click="toggleChangeDesigner()">
        Alterar
      </b-button>
      <br>
      <span v-if="showChangeDesigner">
        <b-form style="display:inline-block">
          <b-form-select
            id="designer-select"
            v-model.trim="designer"
            type="text"
            required
            value-field="name"
          >
            <template v-slot:first>
              <option :value="null" disabled>
                --- Please select the Designer ---
              </option>
            </template>
            <template v-for="des in designers">
              <option :key="des.name" :value="des.name">
                {{ des.name }}
              </option>
            </template>
          </b-form-select>
        </b-form>
        <b-button @click.prevent="editDesigner" class="btn btn-success">
          Change Designer
        </b-button>
      </span>
      <br><br>
    </span>

    <h6>Project Structures :</h6>
    <b-table striped over :items="structures" :fields="fields">
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-info" :to="`/projects/${projectName}/structures/${row.item.name}`">
          Details
        </nuxt-link>
        <b-button class="btn btn-danger" @click="deleteStructure(row.item.name)">
          Delete
        </b-button>
      </template>
    </b-table>
    <nuxt-link to="/projects">
      Go Back
    </nuxt-link>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      project: {},
      designers: [],
      designer: null,
      showChangeDesigner: false,
      fields: ['name', 'project', 'actions']
    }
  },
  computed: {
    projectName () {
      return this.$route.params.name
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
  },
  methods: {
    editDesigner () {
      this.$axios.$put(`api/projects/${this.project.name}/designers/${this.designer}`)
        .then(() => this.$axios.$get(`/api/projects/${this.name}`))
        .then((project) => {
          this.project = project || {}
        })
    },
    toggleChangeDesigner () {
      this.$axios.$get('/api/designers/')
        .then((designers) => {
          this.designers = designers
        })
      this.showChangeDesigner = !this.showChangeDesigner
    },
    deleteStructure (structureName) {
      this.$axios.$delete(`/api/projects/${this.project.name}/structures/` + structureName)
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
