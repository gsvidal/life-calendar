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
  (event: "setIsFormSubmitted", value: boolean): void;
  (event: "setDate", value: DateObj): void;
}>();

const areInputsValidated = () => {
  return localDate.value.day && localDate.value.month && localDate.value.year;
};

const handleSubmit = (event: Event) => {
  event.preventDefault();
  if (areInputsValidated()) {
    emit("setIsFormSubmitted", true);
  }
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
        <label for="day">Day :</label>
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
        <label for="month">Month :</label>
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
        <label for="year">Year :</label>
        <input
          id="year"
          type="number"
          name="year"
          v-model="localDate.year"
          @input="handleInput"
        />
      </div>
      <button class="button-6" :class="{ disabled: !areInputsValidated() }">
        Create your <strong>life Calendar</strong>
      </button>
      <p v-if="!areInputsValidated()" class="error-msg">
        All fields must be filled
      </p>
      <p v-else></p>
    </form>
  </div>
</template>

<style scoped>
.birthday-form__container {
  position: absolute;
  top: 80px;
  left: calc(50vw - 122px);
  z-index: 3;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  min-width: 230px;
  max-width: 400px;
  padding: 30px;
  border: 1px solid rgb(219, 219, 219);
  border-radius: 5px;
  backdrop-filter: blur(10px);
  background-color: rgba(255, 255, 255, 0.1);
  box-shadow: rgba(255, 255, 255, 0.35) 0px 5px 15px;
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
  margin: 20px 0;
}
.input__container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 180px;
  margin: 8px 0;
}
label {
  text-align: left;
  display: inline-block;
  width: 90px;
  font-size: 1.5rem;
}
input {
  width: 70px;
  padding: 2px 0px 2px 8px;
  border: 1px solid rgb(219, 219, 219);
  border-radius: 4px;
  font-size: 1.1rem;
}
.error-msg {
  color: rgb(255, 148, 148);
  margin-top: 10px;
  font-size: 1.3rem;
}
button.disabled {
  color: rgb(192, 192, 192);
  background-color: grey;
}
button.disabled:hover {
  transform: none;
  cursor: initial;
}
</style>
