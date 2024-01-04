<script lang="ts">

const start = new Date()
const defaultForm = {
  day: start.getDay(),
  month: start.getMonth() + 1,
  year: start.getFullYear()
}

export default {
  emits: ['dateDone'],
  data() {
    return {
      form: { ...defaultForm },
      errors: {} as Record<string, string>

    }
  },
  methods: {
    updateForm() {
      this.errors = {}
    },
    submitForm() {
      // Reset errors
      this.errors = {}

      // Validation
      if (!this.form.day || !this.form.month || !this.form.year) {
        this.errors = { ...this.errors, day: 'Please fill in all fields.' }
        return
      }

      const currentDate = new Date()
      const enteredDate = new Date(this.form.year, this.form.month - 1, this.form.day)

      if (enteredDate > currentDate) {
        this.errors = { ...this.errors, day: 'Date cannot be in the future.' }
        return
      }

      // Additional validation logic if needed
      const endDate = new Date(this.form.year, this.form.month-1, this.form.day)
      const diff = currentDate.getTime() - endDate.getTime()
      const seconds = Math.floor(diff / 1000)
      const minutes = Math.floor(seconds / 60)
      const hours = Math.floor(minutes / 60)
      const days = Math.floor(hours / 24)
      const month = Math.floor(days/30)
      const year = Math.floor(month/12)
      const response = { seconds, minutes, hours, days, month, year }
      this.$emit('dateDone', response)

    },
    incrementDay() {
      const maxDaysInMonth = new Date(this.form.year, this.form.month, 0).getDate()

      if (this.form.day < maxDaysInMonth) {
        this.form.day++
      } else {
        this.form.day = 1
        this.incrementMonth()
      }

      this.updateForm()
    },
    decrementDay() {
      if (this.form.day > 1) {
        this.form.day--
      } else {
        this.decrementMonth()
        this.form.day = new Date(this.form.year, this.form.month, 0).getDate()
      }
      this.updateForm()
    },
    incrementMonth() {
      if (this.form.month < 12) {
        this.form.month++
      } else {
        this.form.month = 1
        this.incrementYear()
      }
    },
    decrementMonth() {
      if (this.form.month > 1) {
        this.form.month--
      } else {
        this.form.month = 12
        this.decrementYear()
      }
    },
    incrementYear() {
      this.form.year++
    },
    decrementYear() {
      this.form.year--
    }
  }
}
</script>

<template>
  <form class="form-input">
    <div class="input" :class="{ 'error': errors.day }">
      <span>Day</span>
      <input min="1" max="31" v-model="form.day" type="number" />
      <span v-if="errors.day" class="error-message">{{ errors.day }}</span>
    </div>
    <div class="input" :class="{ 'error': errors.month }">
      <span>Month</span>
      <input min="1" max="12" v-model="form.month" type="number" />
      <span v-if="errors.month" class="error-message">{{ errors.month }}</span>
    </div>
    <div class="input" :class="{ 'error': errors.year }">
      <span>Year</span>
      <input min="1800" max="2024" v-model="form.year" type="number" />
      <span v-if="errors.year" class="error-message">{{ errors.year }}</span>
    </div>
  </form>
  <div class="form-buttons">
    <button @click.prevent="incrementDay">+ 1 Day</button>
    <button @click.prevent="decrementDay">- 1 Day</button>
  </div>
  <div class="submit-form">
    <div class="button" @click.prevent="submitForm"></div>
  </div>
</template>


<style scoped>
.input {
  margin-bottom: 15px;
  position: relative;
}

.error {
  border-color: #ff5a5a;
}

.error-message {
  color: #ff5a5a;
  font-size: 12px;
  position: absolute;
  top: 100%;
  left: 0;
}

button {
  padding: 10px 15px;
  background-color: hsl(259, 100%, 65%);
  color: #fff;
  cursor: pointer;
  border: none;
  border-radius: 5px;
}

button:hover,
button:focus {
  background-color: #45a049;
}
</style>