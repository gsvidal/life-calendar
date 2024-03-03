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
  emit("setDate", { ...localDate.value, [target.name]: target.value });
};
</script>

<template>
  <div class="birthday-form__container">
    <h1>Enter your Date of Birth:</h1>

    <form @submit="handleSubmit">
      <div class="input__container">
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
      </div>
      <div class="input__container">
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
      </div>
      <div class="input__container">
        <label for="year">Year</label>
        <input
          id="year"
          type="number"
          name="year"
          v-model="localDate.year"
          @input="handleInput"
        />
      </div>
      <button class="button-6">Create Calendar</button>
    </form>
  </div>
</template>

<style scoped>
.birthday-form__container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  width: 50%;
  padding: 30px;
  margin: 0 auto;
  margin-top: 30px;
  border: 1px solid rgb(219, 219, 219);
  border-radius: 5px;
  backdrop-filter: blur(10px);
  background-color: rgba(255, 255, 255, 0.192);
}
h1 {
  display: inline-block;
  font-size: 1.7rem;
  margin-bottom: 15px;
}
form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.input__container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 8px 0;
}
label {
  text-align: left;
  display: inline-block;
  width: 60px;
  font-size: 1.5rem;
}
input {
  width: 70px;
  padding: 2px 8px;
  border: 1px solid rgb(219, 219, 219);
  border-radius: 4px;
  font-size: 1.1rem;
}
/* CSS */
.button-6 {
  align-items: center;
  background-color: #ffffff;
  border: 1px solid rgba(0, 0, 0, 0.1);
  border-radius: 0.25rem;
  box-shadow: rgba(0, 0, 0, 0.02) 0 1px 3px 0;
  box-sizing: border-box;
  color: rgba(0, 0, 0, 0.85);
  cursor: pointer;
  display: inline-flex;
  font-family: system-ui, -apple-system, system-ui, "Helvetica Neue", Helvetica,
    Arial, sans-serif;
  font-size: 1.3rem;
  font-weight: 600;
  justify-content: center;
  line-height: 1.25;
  margin-top: 20px;
  min-height: 3rem;
  padding: calc(0.875rem - 1px) calc(1.5rem - 1px);
  position: relative;
  text-decoration: none;
  transition: all 250ms;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  vertical-align: baseline;
  width: 150px;
}

.button-6:hover,
.button-6:focus {
  border-color: rgba(0, 0, 0, 0.15);
  box-shadow: rgba(0, 0, 0, 0.1) 0 4px 12px;
  color: rgba(0, 0, 0, 0.65);
}

.button-6:hover {
  transform: translateY(-1px);
}

.button-6:active {
  background-color: #f0f0f1;
  border-color: rgba(0, 0, 0, 0.15);
  box-shadow: rgba(0, 0, 0, 0.06) 0 2px 4px;
  color: rgba(0, 0, 0, 0.65);
  transform: translateY(0);
}
</style>
