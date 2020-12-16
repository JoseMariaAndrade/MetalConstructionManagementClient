<template>
  <b-container>
    <h3>Login into Metal Construction Management</h3>
    <b-form @submit.prevent="onSubmit" @reset="onReset">
      <b-form-group label="Name" description="Enter your name">
        <b-input
          v-model.trim="name"
          name="name"
          placeholder="Your name"
          required
        />
      </b-form-group>
      <b-form-group label="Password" description="Enter your password">
        <b-input
          v-model="password"
          name="password"
          type="password"
          placeholder="Your password"
          required
        />
      </b-form-group>
      <b-button type="reset" class="btn-warning">
        Reset
      </b-button>
      <b-button type="submit" class="btn-success">
        Submit
      </b-button>
    </b-form>
  </b-container>
</template>

<script>
export default {
  auth: false,
  data () {
    return {
      name: null,
      password: null
    }
  },
  methods: {
    onLogin () {
      this.$toast.success('You are logged in!').goAway(3000)

      console.log(this.$auth.loggedIn)
      const user = this.$auth.user
      const groups = user.groups

      const role = groups[0]

      if (role === 'Client') {
        this.$router.push(`/clients/${user.sub}`)
      } else if (role === 'Administrator') {
        this.$router.push('/administrator')
      } else if (role === 'Designer') {
        this.$router.push(`/designers/${user.sub}`)
      } else if (role === 'Manufacturer') {
        this.$router.push(`/manufacturers/${user.sub}`)
      } else {
        this.$router.push('/')
      }
    },
    onSubmit () {
      const promise = this.$auth.loginWith('local', {
        data: {
          name: this.name,
          password: this.password
        }
      })

      promise.then(this.onLogin)
    },
    onReset () {
      this.name = null
      this.password = null
    }
  }
}
</script>
