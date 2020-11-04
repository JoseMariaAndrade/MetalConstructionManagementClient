<template>
  <div>
    <b-form @submit.prevent="create">
      <b-form-group id="description-input-group" label-for="description-input" label="Description:">
        <b-form-input id="description-input" v-model="description" type="text" />
      </b-form-group>
    </b-form>
    <nuxt-link to="/types">
      Return
    </nuxt-link>
    <b-button variant="warning" type="reset">
      RESET
    </b-button>
    <b-button variant="success" @click.prevent="create">
      CREATE
    </b-button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      description: null
    }
  },
  methods: {
    create () {
      this.$axios.$post('/api/types', {
        description: this.description
      }).then(() => {
        this.$router.push('/types')
      })
    }
  }
}
</script>

<style scoped>

</style>
