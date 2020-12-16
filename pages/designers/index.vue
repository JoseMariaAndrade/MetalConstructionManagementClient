<template>
  <b-container>
    <b-table striped over :items="designers" :fields="fields">
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" :to="`/designers/${row.item.id}`">
          Details
        </nuxt-link>
        <nuxt-link class="btn btn-link" :to="`/designers/${row.item.id}/edit`">
          Edit
        </nuxt-link>
      </template>
    </b-table>
    <nuxt-link to="/designers/create">
      Create a New Designer
    </nuxt-link>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      fields: ['name', 'email', 'actions'],
      designers: []
    }
  },
  created () {
    this.$axios.$get('/api/designers')
      .then((designers) => {
        this.designers = designers
      })
  }
}
</script>

<style scoped>

</style>
