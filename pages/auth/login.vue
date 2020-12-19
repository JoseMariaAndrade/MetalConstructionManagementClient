<template>
  <b-container>
    <b-form @submit.prevent="onSubmit" @reset="onReset">
      <b-form-group label="E-mail" description="Enter your e-mail">
        <b-input
          v-model.trim="email"
          name="email"
          type="email"
          placeholder="Your E-mail"
          required
        />
      </b-form-group>
      <b-form-group label="Password" description="Enter your password">
        <b-input
          v-model="password"
          name="password"
          type="password"
          placeholder="Your Password"
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
      email: null,
      password: null
    }
  },
  methods: {
    onSubmit () {
      const promise = this.$auth.loginWith('local', {
        data: {
          email: this.email,
          password: this.password
        }
      })
      promise.then(() => {
        this.$toast.success('You are logged in!').goAway(1000)
        const userID = this.$auth.user.subID
        if (this.$auth.user.groups.includes('Client')) {
          this.$router.push(`/clients/${userID}`)
        } else if (this.$auth.user.groups.includes('Designer')) {
          this.$router.push(`/designers/${userID}`)
        } else if (this.$auth.user.groups.includes('Manufacturer')) {
          this.$router.push(`/manufacturers/${userID}`)
        } else if (this.$auth.user.groups.includes('Administrator')) {
          this.$router.push('/families')
        }
      })
      promise.catch(() => {
        this.$toast.error('Sorry, you cant login. Ensure your credentials are correct').goAway(2000)
      })
    },
    onReset () {
      this.email = null
      this.password = null
    }
  }
}
</script>
<style scoped>

</style>
