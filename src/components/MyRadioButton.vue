<script setup>
const { modelValue, id, value } = defineProps({
  modelValue: String | Boolean,
  id: { type: String, required: true },
  name: String,
  value: String,
});
</script>

<template>
  <input
    type="radio"
    :class="$style.input"
    :value="modelValue"
    :checked="modelValue === value"
    @change="$emit('update:modelValue', $event.target.value)"
    v-bind="$props"
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
