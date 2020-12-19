<template>
  <b-container>
    <h4>Structure Details:</h4>
    <p>Name: {{ structure.name }}</p>
    <p>Project: {{ structure.project }} </p>

    <h6>Structure Products :</h6>
    <b-table
      v-if="structure.products"
      striped
      hover
      head-variant="dark"
      sticky-header="400px"
      responsive="md"
      :items="structure.products"
      :fields="fields">
      <template v-slot:cell(actions)="row">
        <nuxt-link
          class="btn btn-info"
          :to="`/projects/${projectName}/structures/${structureName}/products/${row.item.name}`"
        >
          Details
        </nuxt-link>
        <b-button class="btn btn-danger" @click="deleteStructure(row.item.name)">
          Delete
        </b-button>
      </template>
    </b-table>
    <div v-else>
      <h6>A estrutura não tem produtos associados</h6>
    </div>
    <nuxt-link :to="`/designers/${id}/project/${projectName}`">
      Go Back
    </nuxt-link>
    <nuxt-link :to="`/designers/${id}/project/${projectName}/structure/${structureName}/simulation`">
      Simulation
    </nuxt-link>
  </b-container>
</template>
<script>
export default {
  auth: true,
  data () {
    return {
      structure: {},
      fields: ['name', 'type', 'family']
    }
  },
  computed: {
    id () {
      return this.$auth.user.subID
    },
    projectName () {
      return this.$route.params.name
    },
    structureName () {
      return this.$route.params.structureName
    }
  },
  created () {
    this.$axios.$get(`/api/structures/${this.structureName}`)
      .then((structure) => {
        this.structure = structure || {}
      })
  }
}
</script>
<style>

</style>
