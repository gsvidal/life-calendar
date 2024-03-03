<script setup lang="ts">
import { ref } from "vue";
import { DateObj } from "../App.vue";

const props = defineProps<{ date: DateObj }>();
const localDate = ref(props.date)
const weeks = ref<number>(52);
const years = ref<number>(80);

const todayDate = new Date();
const birthdayDate = new Date(+localDate.value.year, +localDate.value.month - 1, +localDate.value.day);

function yearsBetweenDates(date1: Date, date2: Date) {
  const differenceInMilliseconds = Math.abs(date1.getTime() - date2.getTime());
  const years = Math.floor(
    differenceInMilliseconds / (1000 * 60 * 60 * 24 * 365)
  );
  return years;
}
console.log(yearsBetweenDates(todayDate, birthdayDate));
function weeksBetweenDates(date1: Date, date2: Date) {
  const differenceInMilliseconds = Math.abs(date1.getTime() - date2.getTime());
  const weeks = Math.floor(
    differenceInMilliseconds / (1000 * 60 * 60 * 24 * 7)
  );
  return weeks;
}

const passedWeeks = ref(weeksBetweenDates(todayDate, birthdayDate));
const roundedWeeks = ref(
  Math.floor((365 / 7 - 52) * yearsBetweenDates(todayDate, birthdayDate))
);
console.log("rounded weeks: ", roundedWeeks.value);
</script>

<template>
  <div class="container">
    <h1>Calendar:</h1>
    <h4>Birthday: {{ localDate.day }}/{{ localDate.month }}/{{ localDate.year }}</h4>

    <div class="title__container">
      <p v-for="week in weeks" class="title title--week">{{ week }}</p>
      <p v-for="year in years + 1" class="title title--year">{{ year - 1 }}</p>
    </div>
    <div class="calendar__container">
      <div v-for="(_, index) in (years + 1) * weeks">
        <div
          :class="{
            calendar__block: true,
            passedSinceBirthday: index < passedWeeks - roundedWeeks + 1,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 350vw;
}
</style>
