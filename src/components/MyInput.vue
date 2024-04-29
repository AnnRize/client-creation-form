<script>
export default {
  inheritAttrs: false,
};
</script>

<script setup>
const model = defineModel();
</script>

<template>
  <input :class="$style.input" v-model="model" v-bind="$attrs" />
</template>

<style module>
.input {
  background-color: var(--primary);
  color: var(--secondary);
  padding: 5px;
  outline: none;
  border-radius: 5px;
  border: none;
  font-size: 1.1rem;
  width: 100%;
  min-width: 200px;

  &[type="date"]::-webkit-calendar-picker-indicator {
    background-image: url("../assets/calendar2.svg");
  }

  &:focus {
    outline: 2px solid white;
  }
}
</style>
