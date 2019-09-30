<template>
  <div>
    <form @submit.prevent="submit">
      <input
        type="email"
        placeholder="What's your email"
        v-model="email"
        @blur="$v.email.$touch()"
        :class="{ error: $v.email.$error }"
      />
      <div v-if="$v.email.$error">
        <p v-if="!$v.email.required" class="-text-error">Email is required.</p>
        <p v-if="!$v.email.email" class="-text-error">Please input a valid email.</p>
      </div>
      <button type="submit" :disabled="$v.$invalid">Submit!</button>
      <p v-if="$v.$anyError" class="-text-error">Please fill-out required fields.</p>
    </form>
  </div>
</template>

<script>
import { email, required } from 'vuelidate/lib/validators'
export default {
  data() {
    return {
      email: null
    }
  },
  validations: {
    email: {
      email,
      required
    }
  },
  methods: {
    submit() {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        alert('Submitted!')
      }
    }
  }
}
</script>