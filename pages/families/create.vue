<template>
  <div>
    <b-form @submit.prevent="create">
      <b-form-group id="name-input-group" label-for="name-input" label="Name:">
        <b-form-input id="name-input" v-model="name" type="text" />
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
      name: null
    }
  },
  methods: {
    create () {
      this.$axios.$post('/api/families', {
        name: this.name
      }).then(() => {
        this.$router.push('/families')
      })
    }
  }
}
</script>

<style scoped>

</style>
