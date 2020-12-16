<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">
        MetalConstructionManagementClient
      </h1>
      <div class="links">
        <nuxt-link
          class="button--green"
          to="/projects"
        >
          Projects
        </nuxt-link><nuxt-link
          class="button--green"
          to="/structures"
        >
          Structures
        </nuxt-link>
        <nuxt-link
          class="button--green"
          to="/clients"
        >
          Clients
        </nuxt-link>
        <nuxt-link
          class="button--green"
          to="/documents"
        >
          Documents
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style>
.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 50px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
