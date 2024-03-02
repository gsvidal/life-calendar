<script setup lang="ts">
import { ref, onMounted } from "vue";
import BirthdayForm from "./components/BirthdayForm.vue";
import Calendar from "./components/Calendar.vue";

const isFormSubmitted = ref<boolean>(false);

export type DateObj = {
  day: string;
  month: string;
  year: string;
};
const date = ref<DateObj>({
  day: "17",
  month: "11",
  year: "1995",
});
</script>

<template>
  <div v-if="!isFormSubmitted">
    <BirthdayForm
      :date="date"
      @setIsFormSubmitted="(value: boolean) => (isFormSubmitted = value)"
      @setDate="(value: DateObj) => (date = {...value})"
    />
    <p>Date day: {{ date.day }}</p>
    <p>Date month: {{ date.month }}</p>
    <p>Date year: {{ date.year }}</p>
  </div>
  <div v-else="">
    <Calendar :date="date" />
  </div>
</template>

<style scoped></style>
