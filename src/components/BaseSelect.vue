<template>
  <div>
    <label v-if="label">{{ label }}</label>
    <select :input="value" @change="updateValue" v-bind="$attrs" v-on="$listeners">
      <option value>Select</option>
      <option v-for="option in options" :key="option" :selected="option === value">{{ option }}</option>
    </select>
  </div>
</template>

<script>
import { formFieldMixin } from '../mixins/formFieldMixin'
export default {
  mixins: [formFieldMixin],
  props: {
    options: {
      type: Array,
      required: true
    }
  }
}
</script>