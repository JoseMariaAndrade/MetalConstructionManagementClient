<template>
  <b-container>
    <h4>Project Details:</h4>
    <p>Name: {{ project.name }}</p>
    <p>Client: {{ project.nameClient }}</p>
    <p>Designer: {{ project.nameDesigner }}</p>
    <p>
<!--    <p v-show="showDecision">-->
      Decision: {{ project.decision }}
    </p>
    <p>
<!--    <p v-show="showDecision">-->
      Observation: {{ project.observation }}
    </p>
    <form @submit.prevent="create">
      <b-form-file v-model="formData.file" :state="Boolean(formData.file)" placeholder="Choose a file or drop it here..." drop-placeholder="Drop file here..." />
      <div class="mt-3">
        Selected file: {{ formData.file ? formData.file.name : '' }}
      </div>
      <b-button variant="success" @click.prevent="create">
        CREATE
      </b-button>
    </form>
    <h4>Structures:</h4>
    <b-table
      v-if="structures.length"
      striped
      hover
      :items="structures"
      :fields="structuresFields"
    >
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" :to="`/clients/${id}/project/${name}/structure/${row.item.name}`">
          Details
        </nuxt-link>
      </template>
    </b-table>
    <p v-else>
      No Structures.
    </p>
    <nuxt-link class="btn btn-link" :to="`/clients/${id}/project/${name}/decision`">
      Details
    </nuxt-link>
    <nuxt-link :to="`/clients/${id}`">
      Go Back
    </nuxt-link>
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

    console.log(this.project)
  },
  methods: {
    create () {
      console.log(this.formData)
      console.log(this.name)
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
          // this.$router.go(0)
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
