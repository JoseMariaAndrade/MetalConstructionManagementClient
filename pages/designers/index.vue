<template>
  <div>
    <b-container>
      <b-table striped over :items="designers" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/designers/${row.item.name}`">
            Details
          </nuxt-link>
          <nuxt-link class="btn btn-link" :to="`/designers/${row.item.name}/edit`">
            Edit
          </nuxt-link>
        </template>
      </b-table>
    </b-container>
    <nuxt-link to="/designers/create">
      Create a New Designer
    </nuxt-link>
  </div>
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
