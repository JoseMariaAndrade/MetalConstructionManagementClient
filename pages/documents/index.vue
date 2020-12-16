<template>
  <b-container>
    <h3>Document List</h3>
    <b-table striped hover :items="documents" :fields="fields">
      <template v-slot:cell(actions)="row">
        <b-button class="btn btn-link" target="_blank" @click.prevent="download(row.item)">
          Download
        </b-button>
      </template>
    </b-table>

    <form @submit.prevent="upload">
      <!-- Styled -->
      <b-form-file
        v-model="file"
        :state="hasFile"
        placeholder="Choose a file or drop it here..."
        drop-placeholder="Drop file here..."
      />
      <div class="mt-3">
        Selected file: {{ file ? file.name : '' }}
      </div>

      <b-button type="submit" :disabled="!hasFile">
        Upload
      </b-button>
    </form>
  </b-container>
</template>
<script>
export default {
  auth: false,
  data () {
    return {
      name: this.$auth.user.sub,
      file: null,
      documents: [],
      fields: ['fileName', 'projectName']
    }
  },
  created () {
  },
  hasFile () {
    return this.file != null
  },
  formData () {
    const formData = new FormData()

    formData.append('username', this.$auth.user.sub)

    if (this.file) {
      formData.append('file', this.file)
    }

    return formData
  },
  methods: {
    upload () {
      if (!this.hasFile) {
        return
      }

      const promise = this.$axios.$post('/api/documents/upload', this.formData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      })

      promise.then(() => {
        this.$toast.success('File uploaded!').goAway(3000)
      })
    },
    download (fileToDownload) {
      const documentId = fileToDownload.id

      this.$axios.$get('/api/documents/download/' + documentId, { responseType: 'arraybuffer' })
        .then((file) => {
          const url = window.URL.createObjectURL(new Blob([file]))
          const link = document.createElement('a')
          link.href = url
          link.setAttribute('download', fileToDownload.filename)
          document.body.appendChild(link)
          link.click()
        })
    }
  }
}
</script>
<style>

</style>
