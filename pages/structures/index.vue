<template>
  <b-container>
    <b-table
      striped
      hover
      head-variant="dark"
      sticky-header="400px"
      responsive="md"
      :items="structures"
      :fields="fields"
    />
  </b-container>
</template>

<script>
export default {
  auth: true,
  data () {
    return {
      fields: ['name', 'nb', 'q'],
      structures: []
    }
  },
  created () {
    this.$axios.$get('/api/structures')
      .then((structures) => {
        this.structures = structures || {}
      })
  }
}
</script>

<style scoped>

</style>
