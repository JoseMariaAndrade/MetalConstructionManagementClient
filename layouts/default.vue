<template>
  <div>
    <div>
      <b-navbar toggleable="lg" type="dark" variant="dark">
        <b-navbar-brand>
          Metal Construction Management
        </b-navbar-brand>

        <b-navbar-toggle target="nav-collapse" />

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item v-if="!$auth.$state.loggedIn" href="/auth/login">
              Login
            </b-nav-item>
            <b-nav-item v-if="$auth.$state.loggedIn" @click="signOut">
              Logout
            </b-nav-item>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>
    <div>
      <Nuxt />
    </div>
  </div>
</template>
<script>
export default {
  methods: {
    signOut () {
      this.$auth.logout()
      this.$router.push('/')
    }
  }
}
</script>
<style>
html {
  font-family:
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}
</style>
