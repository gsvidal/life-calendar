<script setup lang="ts">
import { ref } from "vue";
import { DateObj } from "../App.vue";

defineProps<{ date: DateObj }>();
</script>

<template>
  <h1>Calendar:</h1>
  <p>Date day: {{ date.day }}</p>
  <p>Date month: {{ date.month }}</p>
  <p>Date year: {{ date.year }}</p>
</template>

<style scoped></style>
