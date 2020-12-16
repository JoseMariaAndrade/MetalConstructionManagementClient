<template>
  <b-container>
    <h4>Project Details:</h4>
    <p>Name: {{ project.name }}</p>
    <p>Client: {{ project.nameClient }}</p>
    <p>Designer: {{ project.nameDesigner }}</p>
    <p>
      Decision:
      <b-icon-question-circle-fill v-if="project.decision==null" />
      <b-icon-check-circle-fill v-if="project.decision===true" variant="success" />
      <b-icon-x-circle-fill v-if="project.decision===false" variant="danger" />
    </p>
    <p>
      Observation: {{ project.observation }}
    </p>
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
    <h4>Documents</h4>
    <b-table
      v-if="documents.length"
      striped
      hover
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
          <b-icon-download v-if="row.item!=null" />
        </b-button>
        <b-icon-file-earmark-break-fill v-else />
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
    <nuxt-link class="btn btn-link" :to="`/clients/${id}/project/${name}/decision`">
      Details
    </nuxt-link>
    <nuxt-link :to="`/clients/${id}`">
      Go Back
    </nuxt-link>
  </b-container>
</template>
<script>
import { BIconCheckCircleFill, BIconXCircleFill, BIconQuestionCircleFill, BIconDownload, BIconFileEarmarkBreakFill } from 'bootstrap-vue'
export default {
  components: {
    BIconCheckCircleFill,
    BIconXCircleFill,
    BIconQuestionCircleFill,
    BIconDownload,
    BIconFileEarmarkBreakFill
  },
  auth: false,
  data () {
    return {
      project: {},
      file: null,
      documents: {},
      documentsFields: ['filename', 'actions'],
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
      })
      promisse.catch(() => {
        this.$toast.error('Sorry, could no upload file!').goAway(3000)
      })
      this.documents.push({ filename: this.file.name })
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
    }
  }
}
</script>

<style scoped>

</style>
