<template>
  <div>
    <b-container>
      <b-table striped over :items="clients" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/clients/${row.item.name}`">
            Details
          </nuxt-link>
        </template>
      </b-table>
    </b-container>
    <nuxt-link to="/clients/create">
      Create a New Client
    </nuxt-link>
  </div>
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
