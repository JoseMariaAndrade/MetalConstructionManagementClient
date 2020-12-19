<template>
  <b-container>
    <b-form-group
      label="Decision About Structure"
    >
      <b-form-radio-group
        v-model="selected"
        :options="options"
        value-field="item"
        text-field="name"
      />
    </b-form-group>
    <b-form-textarea
      id="textarea"
      v-model="formData.observation"
      placeholder="Enter something..."
      rows="3"
      max-rows="6"
    />
    <b-button variant="success" @click.prevent="decision">
      CREATE
    </b-button>
  </b-container>
</template>

<script>
export default {
  auth: true,
  data () {
    return {
      selected: '',
      options: [
        { item: true, name: 'Approve' },
        { item: false, name: 'Deny' }
      ],
      project: {},
      formData: {
        decision: null,
        observation: null
      },
      structuresFields: ['name', 'actions']
    }
  },
  computed: {
    id () {
      return this.$auth.user.subID
    },
    name () {
      return this.$route.params.name
    },
    structures () {
      return this.project.structures || []
    },
    structureName () {
      return this.$route.params.structureName
    }
  },
  created () {
    this.$axios.$get(`/api/clients/${this.id}/project/${this.name}/structure/${this.structureName}`)
      .then((project) => {
        this.project = project || {}
      })
      .catch(() => {
        this.$router.push(`/clients/${this.id}`)
      })
  },
  methods: {
    decision () {
      this.project.decision = this.formData.decision
      this.formData.decision = this.selected
      this.project.observation = this.formData.observation
      this.$axios.$post(`/api/clients/${this.id}/project/${this.name}/structure/${this.structureName}`, this.formData)
        .then(() => {
          this.$axios.$post(`/api/clients/${this.id}/email/send`, {
            project: this.name,
            subject: this.formData.decision,
            message: this.formData.observation
          })
            .then((message) => { this.$toast.success(message).goAway(1000) })
            .then(this.$router.go(-1))
        }).catch((error) => {
          this.errorMessage = error.response.data
        })
    }
    // approve (nameProject) {
    //   this.$axios.$post(`/api/clients/${this.id}/approve/${nameProject}`)
    //   // console.log(nameProject)
    // }
  }
}
</script>

<style scoped>

</style>
