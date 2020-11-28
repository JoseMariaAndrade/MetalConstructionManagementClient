<template>
  <div>
    <b-form @submit.prevent="create">
      <b-form-group
        id="name-input-group"
        description="The name is required"
        label-for="name-input"
        label="Name"
      >
        <b-form-input
          id="name-input"
          v-model.trim="formData.name"
          type="text"
          required
          trim
          placeholder="Enter name"
        />
      </b-form-group>
      <b-form-group id="type-input-group" label="Type:" label-for="type-input">
        <b-form-select
          id="type-input"
          v-model="formData.type"
          required
          value-field="code"
        >
          <template v-slot:first>
            <option :value="null" disabled>
              --- Please select the Type ---
            </option>
          </template>
          <template v-for="type in types">
            <option :key="type.name" :value="type.name">
              {{ type.name }}
            </option>
          </template>
        </b-form-select>
      </b-form-group>
      <b-form-group id="family-input-group" label="Family:" label-for="family-input">
        <b-form-select
          id="family-input"
          v-model="formData.family"
          required
          value-field="code"
        >
          <template v-slot:first>
            <option :value="null" disabled>
              --- Please select the Family ---
            </option>
          </template>
          <template v-for="family in families">
            <option :key="family.name" :value="family.name">
              {{ family.name }}
            </option>
          </template>
        </b-form-select>
      </b-form-group>
    </b-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      formData: {
        name: null,
        type: null,
        family: null,
        manufacturer: null
      },
      types: [],
      families: []
    }
  },
  created () {
    this.$axios.$get('/api/types')
      .then((types) => {
        this.types = types
      })
    this.$axios.$get('/api/families')
      .then((families) => {
        this.families = families
      })
  }
}
</script>

<style scoped>

</style>
