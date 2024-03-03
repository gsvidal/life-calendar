<script setup lang="ts">
import { ref } from "vue";
import { DateObj } from "../App.vue";

const props = defineProps<{ date: DateObj; isFormSubmitted: boolean }>();
const localDate = ref(props.date);
const weeks = ref<number>(52);
const years = ref<number>(80);

const todayDate = new Date();
const birthdayDate = ref(
  new Date(
    +localDate.value.year,
    +localDate.value.month - 1,
    +localDate.value.day
  )
);

function yearsBetweenDates(date1: Date, date2: Date) {
  const differenceInMilliseconds = Math.abs(date1.getTime() - date2.getTime());
  const years = Math.floor(
    differenceInMilliseconds / (1000 * 60 * 60 * 24 * 365)
  );
  return years;
}

function weeksBetweenDates(date1: Date, date2: Date) {
  const differenceInMilliseconds = Math.abs(date1.getTime() - date2.getTime());
  const weeks = Math.floor(
    differenceInMilliseconds / (1000 * 60 * 60 * 24 * 7)
  );
  return weeks;
}

const passedWeeks = ref(weeksBetweenDates(todayDate, birthdayDate.value));

const roundedWeeks = ref(
  Math.floor((365 / 7 - 52) * yearsBetweenDates(todayDate, birthdayDate.value))
);
console.log("rounded weeks: ", roundedWeeks.value);
const emit = defineEmits<{
  (event: "setIsFormSubmitted", value: boolean): void;
}>();

const handleClick = () => {
  emit("setIsFormSubmitted", false);
};
</script>

<template>
  <div class="container">
    <h1>Calendar:</h1>
    <h4>
      Birthday: {{ localDate.day }}/{{ localDate.month }}/{{ localDate.year }}
    </h4>
    <button class="calendar-button button-6" @click="handleClick">
      Try again!
    </button>

    <div class="title__container">
      <p class="horizontal">Weeks</p>
      <p class="vertical">Years</p>
      <div class="legend-container">
        <div class="calendar__block big passedSinceBirthday"></div>
        <span class="legend-text">Weeks you've already lived</span>
        <div class="calendar__block big"></div>
        <span class="legend-text">Weeks that haven't passed yet</span>
        <p>If you live until 80 years old, you've already lived <strong class="percentage">{{ (passedWeeks / (80*52) * 100).toFixed(0) }}%</strong> of your expected life span</p>
      </div>
      <p v-for="week in weeks" class="title title--week">{{ week }}</p>
      <p v-for="year in years + 1" class="title title--year">
        {{ year - 1 }}
      </p>
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
  margin-bottom: 80px;
}

.calendar__container {
  display: grid;
  position: absolute;
  top: 323px;
  left: 70px;
  width: fit-content;
  height: fit-content;
  grid-template-columns: repeat(52, 1fr);
  grid-template-rows: repeat(80, 1fr);
  gap: 2px;
  margin: 30px;
}

.calendar__block {
  width: 8px;
  height: 8px;
  border: 1px solid rgb(133, 133, 133);
  font-size: 0.5rem;
  color: rgb(223, 221, 221);
}

.big {
  width: 14px;
  height: 14px;
}

.passedSinceBirthday {
  background-color: rgb(255, 148, 148);
}

.beforeBirthday {
  background-color: grey;
}

.title__container {
  width: 400vw;
}

.title {
  display: inline-block;
  position: relative;
  top: 45px;
  left: 69px;
  width: 8px;
  font-size: 0.3rem;
  margin: 1px;
  text-align: center;
}

.title--year {
  display: flex;
  justify-content: center;
  align-items: center;
  top: 44px;
  left: 57px;
  height: 9px;
}

.calendar-button {
  position: absolute;
  top: 15px;
  left: 270px;
  width: 140px;
  font-size: 1rem;
}

.horizontal {
  position: relative;
  top: 200px;
  left: 260px;
  font-size: 1.8rem;
}
.horizontal::after {
  content: " ----->";
}

.vertical {
  position: absolute;
  top: 600px;
  left: -20px;
  font-size: 1.8rem;
  transform: rotate(-90deg);
}

.vertical::before {
  content: "<----- ";
}

.legend-container {
  width: 290px;
  position: relative;
  top: -10px;
  left: 180px;
  padding: 20px;
  border: 1px solid rgba(136, 136, 136, 0.603);
  border-radius: 5px;
}

.legend-text {
  position: relative;
  top: -16px;
  left: 25px;
}

</style>
