<script setup>
import FormInputs from './FormInputs.vue'
import AgeDisplay from './AgeDisplay.vue'
</script>

<script>

const dateS = {
  days: 0,
  hours: 0,
  minutes: 0,
  month: 0,
  seconds: 0,
  year: 0
}
export default {
  data() {
    return {
      date: { ...dateS }
    }
  },
  methods: {
    onDateDone(value) {
      this.date = value
    }
  }
}
</script>

<template>
  <div class="form">
    <FormInputs @dateDone="onDateDone" />
    <AgeDisplay :dates="date" />
  </div>
</template>

<style scoped>

</style>
