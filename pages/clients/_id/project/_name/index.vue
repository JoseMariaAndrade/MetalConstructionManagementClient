<template>
  <b-container>
    <h4>Project Details:</h4>
    <p>Name: {{ project.name }}</p>
    <p>Client: {{ project.nameClient }}</p>
    <p>Designer: {{ project.nameDesigner }}</p>
    <h4>Structures:</h4>
    <b-col v-show="structures.length" lg="6" class="my-1">
      <b-form-group
        label="Filter"
        label-cols-sm="1"
        label-align-sm="right"
        label-size="sm"
        label-for="filterInput"
        class="mb-0"
      >
        <b-input-group size="sm">
          <b-form-input
            id="filterInput"
            v-model="filterStructures"
            type="search"
            placeholder="Type to Search"
          />
          <b-input-group-append>
            <b-button :disabled="!filterStructures" @click="filterStructures = ''">
              Clear
            </b-button>
          </b-input-group-append>
        </b-input-group>
      </b-form-group>
    </b-col>
    <b-table
      v-if="structures.length"
      :filter="filterStructures"
      striped
      hover
      head-variant="dark"
      sticky-header="800px"
      responsive="md"
      :items="structures"
      :fields="structuresFields"
    >
      <template v-slot:cell(decision)="row">
        <b-icon-question-circle-fill v-if="row.item.decision==null" />
        <b-icon-check-circle-fill v-if="row.item.decision===true" variant="success" />
        <b-icon-x-circle-fill v-if="row.item.decision===false" variant="danger" />
      </template>
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" :to="`/clients/${id}/project/${name}/structure/${row.item.name}`">
          Details
        </nuxt-link>
      </template>

      <template #cell(make_decision)="row">
        <b-button size="sm" class="mr-2" @click="row.toggleDetails">
          {{ row.detailsShowing ? 'Hide' : 'Show' }} Decision
        </b-button>
      </template>
      <template #row-details="row">
        <b-card>
          <b-form-radio-group
            v-model="selected"
            :options="options"
            value-field="item"
            text-field="name"
          />
          <b-form-textarea
            id="textarea"
            v-model="formDataDecision.observation"
            placeholder="Enter something..."
            rows="3"
            max-rows="6"
          />
          <b-button variant="success" @click.prevent="decision(row.item.name)">
            CREATE
          </b-button>
        </b-card>
      </template>
    </b-table>
    <p v-else>
      No Structures.
    </p>
    <h4>Documents</h4>
    <b-col v-show="documents.length" lg="6" class="my-1">
      <b-form-group
        label="Filter"
        label-cols-sm="1"
        label-align-sm="right"
        label-size="sm"
        label-for="filterInput"
        class="mb-0"
      >
        <b-input-group size="sm">
          <b-form-input
            id="filterInput"
            v-model="filterDocuments"
            type="search"
            placeholder="Type to Search"
          />
          <b-input-group-append>
            <b-button :disabled="!filterDocuments" @click="filterDocuments = ''">
              Clear
            </b-button>
          </b-input-group-append>
        </b-input-group>
      </b-form-group>
    </b-col>
    <b-table
      v-if="documents.length"
      :filter="filterDocuments"
      striped
      hover
      head-variant="dark"
      sticky-header="400px"
      responsive="md"
      :items="documents"
      :fields="documentsFields"
    >
      <template v-slot:cell(actions)="row">
        <b-button
          v-if="row.item.id"
          size="sm"
          class="mb-2"
          @click="downloadFile(row.item)"
        >
          <b-icon-download v-if="row.item" />
        </b-button>
      </template>
    </b-table>
    <p v-else>
      No Documents.
    </p>
    <form @submit.prevent="upload">
      <b-form-file
        v-model="file"
        :state="hasFile"
        placeholder="Choose a file or drop it here..."
        drop-placeholder="Drop file here..."
      />
      <div class="mt-3">
        Selected file: {{ file ? file.name : '' }}
      </div>
      <b-button type="submit" variant="success" :disabled="!hasFile">
        Upload
      </b-button>
    </form>
    <nuxt-link :to="`/clients/${id}`">
      Go Back
    </nuxt-link>
  </b-container>
</template>
<script>
import { BIconCheckCircleFill, BIconDownload, BIconQuestionCircleFill, BIconXCircleFill } from 'bootstrap-vue'
export default {
  components: {
    BIconCheckCircleFill,
    BIconXCircleFill,
    BIconQuestionCircleFill,
    BIconDownload
  },
  auth: false,
  data () {
    return {
      selected: '',
      options: [
        { item: true, name: 'Approve' },
        { item: false, name: 'Deny' }
      ],
      project: {},
      file: null,
      documents: {},
      documentsFields: [
        {
          key: 'filename',
          sortable: true
        }, 'actions'],
      structuresFields: [
        {
          key: 'name',
          sortable: true
        },
        {
          key: 'decision',
          sortable: true
        }, 'actions', 'make_decision'],
      filterDocuments: null,
      filterStructures: null,
      formDataDecision: {
        decision: null,
        observation: null
      }
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
    hasFile () {
      return this.file != null
    },
    formData () {
      const formData = new FormData()
      if (this.file) {
        formData.append('attachment', this.file)
      }
      return formData
    }
  },
  created () {
    this.$axios.$get(`/api/clients/${this.id}/project/${this.name}`)
      .then((project) => {
        this.project = project || {}
      }).then(() => this.$axios.$get(`/api/file/${this.name}/documents/`).then((documents) => {
        this.documents = documents || {}
      }))
      .catch(() => {
        this.$router.push(`/clients/${this.id}`)
      })
  },
  methods: {
    upload () {
      if (!this.hasFile) {
        return
      }

      const promisse = this.$axios.$post(`/api/file/${this.name}/upload`, this.formData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      })

      promisse.then(() => {
        this.$toast.success('File uploaded!').goAway(3000)
        this.$axios.$get(`/api/file/${this.name}/documents/`).then((documents) => {
          this.documents = documents || {}
        })
        this.file = null
      })
      promisse.catch(() => {
        this.$toast.error('Sorry, could no upload file!').goAway(3000)
      })
      // this.documents.push({ filename: this.file.name })
    },
    downloadFile (file) {
      const documentId = file.id
      this.$axios.$get(`/api/file/download/${documentId}`, { responseType: 'arraybuffer' })
        .then((filem) => {
          const url = window.URL.createObjectURL(new Blob([filem]))
          const link = document.createElement('a')
          link.href = url
          link.setAttribute('download', file.filename)
          document.body.appendChild(link)
          link.click()
        })
    },
    decision (structureName) {
      this.project.decision = this.selected
      this.formDataDecision.decision = this.selected
      this.project.observation = this.formDataDecision.observation
      this.$axios.$post(`/api/clients/${this.id}/project/${this.project.name}/structure/${structureName}`, this.formDataDecision)
        .then(() => {
          this.$axios.$post(`/api/clients/${this.id}/email/send`, {
            project: this.project.name,
            subject: this.formDataDecision.decision,
            message: this.formDataDecision.observation
          })
            .then((message) => { this.$toast.success(message).goAway(1000) })
            .then(() => {
              this.$axios.$get(`/api/clients/${this.id}/project/${this.name}`)
                .then((project) => {
                  this.project = project || {}
                }).then(() => this.$axios.$get(`/api/file/${this.name}/documents/`).then((documents) => {
                  this.documents = documents || {}
                }))
                .catch(() => {
                  this.$router.push(`/clients/${this.id}`)
                }).then(() => {
                  this.formDataDecision.observation = null
                })
            })
            // .then(this.$router.go(-1))
        }).catch((error) => {
          this.errorMessage = error.response.data
        })
    }
  }
}
</script>

<style scoped>

</style>
