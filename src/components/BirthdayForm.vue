<script setup lang="ts">
import { ref, defineEmits } from "vue";
import { DateObj } from "../App.vue";

// Define props using defineProps
const props = defineProps<{
  date: DateObj;
}>();

const localDate = ref(props.date);

// Define emit with typed arguments and return value
const emit = defineEmits<{
  (event: "setIsFormSubmitted", data: boolean): void;
  (event: "setDate", data: DateObj): void;
}>();

const handleSubmit = (event: Event) => {
  event.preventDefault();
  emit("setIsFormSubmitted", true);
};

const handleInput = (event: Event) => {
  const target = event.target as HTMLInputElement;
  console.log(target.name);
  console.log(target.value);
  emit("setDate", {...localDate.value, [target.name]: target.value})
};
</script>

<template>
  <h1>Your birthday:</h1>

  <form @submit="handleSubmit">
    <label for="day">Day</label>
    <input
      id="day"
      type="number"
      min="1"
      max="31"
      name="day"
      v-model="localDate.day"
      @input="handleInput"
    />
    <label for="month">Month</label>
    <input
      id="month"
      type="number"
      min="1"
      max="12"
      name="month"
      v-model="localDate.month"
      @input="handleInput"
    />
    <label for="year">Year</label>
    <input
      id="year"
      type="number"
      name="year"
      v-model="localDate.year"
      @input="handleInput"
    />
    <input type="submit" value="Continue" />
  </form>
</template>

<style scoped></style>
