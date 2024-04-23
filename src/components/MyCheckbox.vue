<script setup>
const { modelValue, id, $props } = defineProps({
  modelValue: String | Boolean,
  id: { type: String, required: true },
});
</script>

<template>
  <input
    type="checkbox"
    :class="$style.input"
    :checked="modelValue"
    @change="$emit('update:modelValue', $event.target.checked)"
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
