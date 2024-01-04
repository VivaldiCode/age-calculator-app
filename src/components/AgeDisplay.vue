<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'

const props = defineProps<{
  dates?: {
    year?: number;
    month?: number;
    days?: number;
    hours?: number;
    minutes?: number;
    seconds?: number;
  };
}>()

const displaySeconds = ref<number>(0)
const displayMinutes = ref<number>(0)
const displayHours = ref<number>(0)
const updateDisplayValues = () => {
  if (props.dates) {
    displaySeconds.value = props.dates.seconds
  }
}

watch(() => props.dates, () => {
  updateDisplayValues()
})

onMounted(() => {
  setInterval(() => {
    displaySeconds.value++;
  }, 1000);
});
</script>

<template>
  <div>
    <div class="date-line">
      <div class="number">{{ dates?.year }}</div>
      <div class="text"> Years</div>
    </div>
    <div class="date-line">
      <div class="number">{{ dates?.month }}</div>
      <div class="text"> Months</div>
    </div>
    <div class="date-line">
      <div class="number">{{ dates?.days }}</div>
      <div class="text"> Days</div>
    </div>
    <div class="date-line">
      <div class="number">{{ dates?.hours }}</div>
      <div class="text"> Hours</div>
    </div>
    <div class="date-line">
      <div class="number">{{ dates?.minutes }}</div>
      <div class="text"> Minutes</div>
    </div>
    <div class="date-line">
      <div class="number">{{ displaySeconds }}</div>
      <div class="text"> Seconds</div>
    </div>
  </div>
</template>

<style scoped>

</style>