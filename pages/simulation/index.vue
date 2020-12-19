<template>
  <b-container>
    <h3>Simulation</h3>
    <b-button class="btn btn-info" @click="inputSimulation">
      Introduzir dados
    </b-button>
    <b-button class="btn btn-info" @click="selectSimulation">
      Seleccionar estrutura
    </b-button>
    <b-button class="btn btn-info" @click="toggleMaterialsSimulation">
      Gerar lista de materiais compatíveis
    </b-button>
    <div v-if="simulationType === 'input'">
      <b-form @submit.prevent="simulate">
        <b-form-group id="nb-input-group" label-for="nb-input" label="Número de vãos:">
          <b-form-input
            id="nb-input"
            v-model.trim="formData.nb"
            type="number"
            min="1"
            required
            trim
            placeholder="Introduza aqui o número de vãos da estrutura"
          />
        </b-form-group>
        <b-form-group id="lvao-input-group" label-for="lvao-input" label="Comprimento dos vãos (m):">
          <b-form-input
            id="lvao-input"
            v-model.trim="formData.lvao"
            type="number"
            min="0.1"
            required
            trim
            placeholder="Introduza aqui o comprimento de cada vão da estrutura"
          />
        </b-form-group>
        <b-form-group id="q-input-group" label-for="q-input" label="Sobrecarga atuante:">
          <b-form-input
            id="q-input"
            v-model.trim="formData.q"
            type="number"
            min="1"
            max="2147483647"
            required
            trim
            placeholder="Introduza aqui a sobrecarga atuante da estrutura"
          />
        </b-form-group>
        <b-form-group id="variant-input-group" label-for="variant-select" label="Variante:">
          <b-form-select
            id="variant-select"
            v-model.trim="formData.variantCode"
            type="text"
            required
            value-field="name"
          >
            <template v-slot:first>
              <option :value="null" disabled>
                --- Please select the Product Variant ---
              </option>
            </template>
            <template v-for="variant in variants">
              <option :key="variant.codigo" :value="variant.codigo">
                {{ variant.displayName }}
              </option>
            </template>
          </b-form-select>
        </b-form-group>
        <b-button variant="primary" @click.prevent="simulate">
          SIMULATE
        </b-button>
      </b-form>
    </div>
    <div v-if="simulationType === 'select'">
      <b-form @submit.prevent="simulate">
        <b-form>
          <b-form-group id="structure-simulation-group" label-for="structure-simulation-select" label="Seleccione a estrutura a simular">
            <b-form-select
              id="structure-simulation-select"
              v-model.trim="structure"
              type="text"
              required
              value-field="name"
            >
              <template v-slot:first>
                <option :value="null" disabled>
                  --- Please select the Structure ---
                </option>
              </template>
              <template v-for="struct in structures">
                <option :key="struct.name" :value="struct">
                  {{ struct.name }}
                </option>
              </template>
            </b-form-select>
          </b-form-group>
        </b-form>
        <b-button variant="primary" @click.prevent="simulate">
          SIMULATE
        </b-button>
      </b-form>
    </div>
    <div v-if="simulationType === 'materials'">
      <b-form @submit.prevent="simulate">
        <b-form>
          <b-form-group id="structure-materials-group" label-for="structure-materials-select" label="Seleccione a estrutura a simular">
            <b-form-select
              id="structure-materials-select"
              v-model.trim="structure"
              type="text"
              required
              value-field="name"
            >
              <template v-slot:first>
                <option :value="null" disabled>
                  --- Please select the Structure ---
                </option>
              </template>
              <template v-for="struct in structures">
                <option :key="struct.name" :value="struct">
                  {{ struct.name }}
                </option>
              </template>
            </b-form-select>
          </b-form-group>
        </b-form>
        <b-button variant="primary" @click.prevent="simulateMaterials">
          SIMULATE
        </b-button>
      </b-form>
    </div>

    <div v-if="simulationReceived === true && simulationType !== 'materials'">
      <br>
      <div class="badge text-wrap" :class="badgeColor">
        <pre style="color: white; font-size: large">{{ simulationResultText }}</pre>
      </div>
      <br>
      <b-button variant="outline-primary" @click="clear">
        Clear
      </b-button>
    </div>
    <div v-if="simulationReceived === true && simulationType === 'materials'">
      <br>
      <div class="badge text-wrap" :class="badgeColorMaterials">
        <pre style="color: white; font-size: large">{{ materialsSimulationResult }}</pre>
      </div>
      <br>
      <b-button variant="outline-primary" @click="clear">
        Clear
      </b-button>
    </div>
  </b-container>
</template>
<script>
export default {
  data () {
    return {
      variants: [],
      structures: [],
      structure: {},
      simulationResult: null,
      simulationReceived: null,
      simulationType: true,
      materialsSimulation: false,
      materialsSimulationResult: '',
      materialsSimulationResultText: '',
      formData: {
        nb: null,
        lvao: null,
        q: null,
        variantCode: null
      }
    }
  },
  computed: {
    badgeColor () {
      return this.simulationResult === 'seguro' ? 'badge-success' : 'badge-danger'
    },
    simulationResultText () {
      return this.simulationResult === 'seguro' ? 'This variant is safe to use [OK]' : this.simulationResult
    },
    badgeColorMaterials () {
      return this.materialsSimulationResult === '' ? 'badge-danger' : 'badge-success'
    }
  },
  created () {
    this.$axios.$get('/api/variants/')
      .then((variants) => {
        this.variants = variants
      })
      .then(() =>
        // TODO alterar para só as estruturas do projeto em análise pelo Designer
        this.$axios.$get('/api/projects/JoseProjeto/')
          .then((response) => {
            this.structures = response.structures
          })
      )
  },
  methods: {
    simulate () {
      if (!this.inputDataToSimulate) {
        this.formData.lvao = this.structure.lvao
        this.formData.nb = this.structure.nb
        this.formData.q = this.structure.q
        this.formData.variantCode = this.structure.variantCode
      }
      this.$axios.$post('/api/simulations/', this.formData)
        .then((simulationResult) => {
          this.simulationResult = simulationResult
          this.simulationReceived = true
        })
    },
    simulateMaterials () {
      this.formData.lvao = this.structure.lvao
      this.formData.nb = this.structure.nb
      this.formData.q = this.structure.q
      this.formData.variantCode = this.structure.variantCode
      this.$axios.$post('/api/simulations/getMaterials', this.formData)
        .then((materialsSimulationResult) => {
          this.materialsSimulationResult = materialsSimulationResult
          if (this.materialsSimulationResult !== '') {
            this.simulationResult = 'seguro'
          }
          this.simulationReceived = true
        })
    },
    clear () {
      this.simulationReceived = false
    },
    inputSimulation () {
      this.simulationType = 'input'
    },
    selectSimulation () {
      this.simulationType = 'select'
    },
    toggleMaterialsSimulation () {
      this.simulationType = 'materials'
    }
  }
}
</script>
