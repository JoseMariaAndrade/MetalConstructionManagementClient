<template>
  <b-container>
    <b-table striped over :items="clients" :fields="fields">
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" :to="`/clients/${row.item.id}`">
          Details
        </nuxt-link>
      </template>
    </b-table>
    <nuxt-link to="/clients/create">
      Create a New Client
    </nuxt-link>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      fields: ['name', 'email', 'contact', 'address', 'actions'],
      clients: []
    }
  },
  created () {
    this.$axios.$get('/api/clients')
      .then((clients) => {
        this.clients = clients
      })
  }
}
</script>

<style scoped>

</style>
