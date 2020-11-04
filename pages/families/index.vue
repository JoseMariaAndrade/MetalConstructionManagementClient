<template>
  <div>
    <b-container>
      <b-table striped hover :items="families" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/types/${row.item.name}`">
            Details
          </nuxt-link>
        </template>
      </b-table>
    </b-container>
    <nuxt-link to="/families/create">
      Create a New Familiy
    </nuxt-link>
  </div>
</template>

<script>
export default {
  data () {
    return {
      fields: ['name', 'actions'],
      families: []
    }
  },
  created () {
    this.$axios.$get('/api/families')
      .then((families) => {
        this.families = families
      })
  }
}
</script>

<style scoped>

</style>
