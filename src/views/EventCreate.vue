<template>
  <div>
    <h1>Create an Event</h1>
    <form @submit.prevent="createEvent">
      <!-- <p class="-text-error" v-if="$v.$anyError">Please fill-out required fields.</p> -->
      <BaseSelect
        class="field"
        :class="{ 'error': $v.event.category.$error }"
        label="Select a category"
        :options="categories"
        v-model="event.category"
        @blur="$v.event.category.$touch()"
      />
      <template v-if="$v.event.category.$error">
        <p v-if="!$v.event.category.required" class="-text-error">Category is required.</p>
      </template>

      <h3>Name & describe your event</h3>

      <BaseInput
        class="field"
        :class="{ 'error': $v.event.title.$error }"
        label="Title"
        type="text"
        placeholder="Add an event title"
        v-model="event.title"
        @blur="$v.event.title.$touch()"
      />
      <template v-if="$v.event.title.$error">
        <p v-if="!$v.event.title.required" class="-text-error">Title is required.</p>
      </template>

      <BaseInput
        class="field"
        :class="{ 'error': $v.event.description.$error }"
        label="Description"
        type="text"
        placeholder="Add a description"
        v-model="event.description"
        @blur="$v.event.description.$touch()"
      />
      <template v-if="$v.event.description.$error">
        <p v-if="!$v.event.description.required" class="-text-error">Description is required.</p>
      </template>

      <BaseInput
        class="field"
        :class="{ 'error': $v.event.location.$error }"
        label="Location"
        type="text"
        placeholder="Add a location"
        v-model="event.location"
        @blur="$v.event.location.$touch()"
      />
      <template v-if="$v.event.location.$error">
        <p v-if="!$v.event.location.required" class="-text-error">Location is required.</p>
      </template>

      <h3>When is your event?</h3>

      <div class="field">
        <label>Date</label>
        <datepicker
          v-model="event.date"
          placeholder="Select a date"
          :input-class="{ 'error': $v.event.date.$error }"
          @opened="$v.event.date.$touch()"
        />
      </div>
      <template v-if="$v.event.date.$error">
        <p v-if="!$v.event.date.required" class="-text-error">Date is required.</p>
      </template>

      <BaseSelect
        class="field"
        :class="{ 'error': $v.event.date.$error}"
        label="Select a time"
        :options="times"
        v-model="event.time"
        @blur="$v.event.time.$touch()"
      />
      <template v-if="$v.event.time.$error">
        <p v-if="!$v.event.time.required" class="-text-error">Category is required.</p>
      </template>

      <BaseButton type="submit" buttonClass="-fill-gradient" :disabled="$v.$invalid">Submit</BaseButton>
    </form>
  </div>
</template>


<script>
import Datepicker from 'vuejs-datepicker'
import { required } from 'vuelidate/lib/validators'
import NProgress from 'nprogress'
export default {
  components: {
    Datepicker
  },
  data() {
    const times = []
    for (let i = 1; i <= 24; i++) {
      times.push(i + ':00')
    }
    return {
      times,
      categories: this.$store.state.categories,
      event: this.createFreshEventObject()
    }
  },
  validations: {
    event: {
      category: { required },
      title: { required },
      description: { required },
      location: { required },
      date: { required },
      time: { required }
    }
  },
  methods: {
    createEvent() {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        NProgress.start()
        this.$store
          .dispatch('event/createEvent', this.event)
          .then(() => {
            this.$router.push({
              name: 'event-show',
              params: { id: this.event.id }
            })
            this.event = this.createFreshEventObject()
          })
          .catch(() => {
            NProgress.done()
          })
      }
    },
    createFreshEventObject() {
      const user = this.$store.state.user.user
      const id = Math.floor(Math.random() * 10000000)
      return {
        id: id,
        user: user,
        category: '',
        organizer: user,
        title: '',
        description: '',
        location: '',
        date: '',
        time: '',
        attendees: []
      }
    }
  }
}
</script>

<style scoped>
.field {
  margin-bottom: 24px;
}
</style>