<script>
export default {
  inheritAttrs: false,
};
</script>

<script setup>
const model = defineModel();
const { id } = defineProps({
  id: { type: String, required: true },
});
</script>

<template>
  <input
    type="radio"
    :id="id"
    :class="$style.input"
    v-model="model"
    v-bind="$attrs"
  />
  <label :for="id" />
</template>

<style module>
.input {
  display: none;

  + label {
    position: relative;
    width: 20px;
    height: 20px;
    border-radius: 5px;
    background-color: var(--primary);

    &:hover {
      cursor: pointer;
    }
  }

  &:checked + label::after {
    content: "";
    position: absolute;
    inset: 5px;
    border-radius: 2px;
    background-color: var(--secondary);
  }
}
</style>
