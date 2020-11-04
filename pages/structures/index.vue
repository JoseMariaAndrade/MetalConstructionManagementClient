<template>
  <div>
    <b-container>
      <b-table striped over :items="structures" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/clients/${row.item.name}`">
            Details
          </nuxt-link>
        </template>
      </b-table>
    </b-container>
    <nuxt-link to="/structures/create">
      Create a New Structure
    </nuxt-link>
  </div>
</template>

<script>
export default {
  data () {
    return {
      fields: ['name', 'client', 'designer', 'actions'],
      structures: []
    }
  },
  created () {
    this.$axios.$get('/api/structures')
      .then((structures) => {
        this.structures = structures
      })
  }
}
</script>

<style scoped>

</style>
