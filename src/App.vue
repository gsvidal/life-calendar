<script setup lang="ts">
import { onUpdated, ref } from "vue";
import BirthdayForm from "./components/BirthdayForm.vue";
import Calendar from "./components/Calendar.vue";

const isFormSubmitted = ref<boolean>(false);

export type DateObj = {
  day: string;
  month: string;
  year: string;
};

const date = ref<DateObj>({
  day: "24",
  month: "1",
  year: "1989",
});
</script>

<template>
  <div class="app-container">
    <div v-if="!isFormSubmitted">
      <div class="bh-container"></div>
      <BirthdayForm
        :date="date"
        @set-is-form-submitted="isFormSubmitted = $event"
        @set-date="date = $event"
      />
      <div class="backdrop"></div>
    </div>
    <div v-else>
      <Calendar
        :date="date"
        :isFormSubmitted="isFormSubmitted"
        @set-is-form-submitted="isFormSubmitted = $event"
      />
    </div>
  </div>
</template>

<style scoped></style>
