<template>
  <b-container>
    <b-col lg="6" class="my-1">
      <b-form-group
        label="Filter"
        label-cols-sm="1"
        label-align-sm="right"
        label-size="sm"
        label-for="filterInput"
        class="mb-0 p-3"
      >
        <b-input-group size="sm">
          <b-form-input
            id="filterInput"
            v-model="filter"
            type="search"
            placeholder="Type to Search"
          />
          <b-input-group-append>
            <b-button :disabled="!filter" @click="filter = ''">
              Clear
            </b-button>
          </b-input-group-append>
        </b-input-group>
      </b-form-group>
    </b-col>
    <b-table
      striped
      hover
      :filter="filter"
      :items="manufacturers"
      :fields="manufacturersFields"
    >
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" :to="`/manufacturers/${row.item.id}`">
          Details
        </nuxt-link>
      </template>
    </b-table>
    <nuxt-link to="/manufacturers/create">
      Create a New Manufacturers
    </nuxt-link>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      manufacturersFields: [
        {
          key: 'name',
          sortable: true
        },
        {
          key: 'email',
          sortable: true
        }, 'actions'],
      manufacturers: [],
      filter: null
    }
  },
  created () {
    this.$axios.$get('/api/manufacturers')
      .then((manufacturers) => {
        this.manufacturers = manufacturers
      })
  }
}
</script>

<style scoped>

</style>
