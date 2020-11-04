<template>
  <div>
    <b-container>
      <b-table striped over :items="types" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/types/${row.item.description}`">
            Details
          </nuxt-link>
        </template>
      </b-table>
    </b-container>
    <nuxt-link to="/types/create">
      Create a New Type
    </nuxt-link>
  </div>
</template>

<script>
export default {
  data () {
    return {
      fields: ['description', 'actions'],
      types: []
    }
  },
  created () {
    this.$axios.$get('/api/types')
      .then((types) => {
        this.types = types
      })
  }
}
</script>

<style scoped>

</style>
