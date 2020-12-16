<template>
  <b-container>
    <b-form-group
      label="Decicion About Project"
    >
      <b-form-radio v-model="formData.decision" name="some-radios" value="true">
        Approve
      </b-form-radio>
      <b-form-radio v-model="formData.decision" name="some-radios" value="false">
        Unapprove
      </b-form-radio>
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
  data () {
    return {
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
      return this.$route.params.id
    },
    name () {
      return this.$route.params.name
    },
    structures () {
      return this.project.structures || []
    }
    // showDecisionInput () {
    //   // return true
    //   return !(this.project.decision === true || this.project.decision === false)
    // },
    // showDecision () {
    //   return !(this.project.observation === '' || this.project.decision == null)
    // }
  },
  created () {
    this.$axios.$get(`/api/clients/${this.id}/project/${this.name}`)
      .then((project) => {
        this.project = project || {}
      })
  },
  methods: {
    create () {
      this.$axios.$post(`/api/file/${this.name}/upload`, this.formData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      })
        .then(() => {
          this.$router.push((`/api/clients/${this.id}/project/${this.name}`))
        })
    },
    decision () {
      this.project.decision = this.formData.decision
      this.project.observation = this.formData.observation
      this.$axios.$post(`/api/clients/${this.id}/project/${this.name}`, this.formData)
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
