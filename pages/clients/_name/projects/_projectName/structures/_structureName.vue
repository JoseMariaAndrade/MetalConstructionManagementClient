<template>
  <b-container>
    <h4>Structure Details:</h4>
    <p>Name: {{ structure.name }}</p>
    <p>Project: {{ structure.project }} </p>

    <h6>Structure Products :</h6>
    <b-table v-if="structure.products" striped over :items="structure.products" :fields="fields">
      <template v-slot:cell(actions)="row">
        <nuxt-link
          class="btn btn-info"
          :to="`/projects/${projectName}/structures/${structureName}/products/${row.item.name}`"
        >
          Details
        </nuxt-link>
      </template>
    </b-table>
    <div v-else>
      <h6>A estrutura não tem produtos associados</h6>
    </div>
    <nuxt-link :to="`/clients/${clientName}/projects/${projectName}`">
      Go Back
    </nuxt-link>
  </b-container>
</template>
<script>
export default {
  data () {
    return {
      structure: {},
      fields: ['family', 'manufacturer', 'name', 'type', 'actions']
    }
  },
  computed: {
    clientName () {
      return this.$route.params.name
    },
    projectName () {
      return this.$route.params.projectName
    },
    structureName () {
      return this.$route.params.structureName
    }
  },
  created () {
    this.$axios.$get(`/api/clients/${this.clientName}/projects/${this.projectName}/structures/${this.structureName}`)
      .then((structure) => {
        this.structure = structure
      })
  }
}
</script>
<style>

</style>
