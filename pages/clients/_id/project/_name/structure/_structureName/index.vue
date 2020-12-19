<template>
  <b-container>
    <h4>Structure Details:</h4>
    <p>Name: {{ structure.name }}</p>
    <p>Project: {{ structure.project }} </p>
    <p>Dimensions </p>
    <p>
      Decision:
      <b-icon-question-circle-fill v-if="structure.decision==null" />
      <b-icon-check-circle-fill v-if="structure.decision===true" variant="success" />
      <b-icon-x-circle-fill v-if="structure.decision===false" variant="danger" />
    </p>
    <p>
      Observation: {{ structure.observation }}
    </p>
    <h4>Structure Products :</h4>
    <b-col v-show="products.length" lg="6" class="my-1">
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
            v-model="filter"
            type="search"
            placeholder="Type to Search"
          />
          <b-input-group-append>
            <b-button :disabled="!filter" @click="filter = ''">
              Clear
            </b-button>
          </b-input-group-append>
        </b-input-group>
      </b-form-group>
    </b-col>
    <div>
      <b-table
        v-if="products.length"
        :filter="filter"
        striped
        hover
        head-variant="dark"
        sticky-header="600px"
        responsive="md"
        :items="products"
        :fields="fields"
      />
      <div v-else>
        <h6>A estrutura não tem produtos associados</h6>
      </div>
    </div>
    <nuxt-link :to="`/clients/${id}/project/${structure.project}`">
      Go Back
    </nuxt-link>
    <nuxt-link class="btn btn-success" :to="`/clients/${id}/project/${structure.project}/structure/${structure.name}/decision`">
      Make Decision
    </nuxt-link>
  </b-container>
</template>
<script>
import { BIconCheckCircleFill, BIconXCircleFill, BIconQuestionCircleFill } from 'bootstrap-vue'
export default {
  components: {
    BIconCheckCircleFill,
    BIconXCircleFill,
    BIconQuestionCircleFill
  },
  auth: true,
  data () {
    return {
      structure: {},
      fields: ['name', 'family', 'type', 'manufacturerName'],
      filter: null,
      formData: {
        decision: null,
        observation: null
      }
    }
  },
  computed: {
    id () {
      return this.$auth.user.subID
    },
    project () {
      return this.$route.params.name
    },
    structureName () {
      return this.$route.params.structureName
    },
    products () {
      return this.structure.products || []
    }
  },
  created () {
    this.$axios.$get(`/api/clients/${this.id}/project/${this.project}/structure/${this.structureName}`)
      .then((structure) => {
        this.structure = structure
      })
  }
}
</script>
<style>

</style>
