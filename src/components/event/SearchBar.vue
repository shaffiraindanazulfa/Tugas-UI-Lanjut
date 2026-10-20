<template>
  <div class="search-bar">
    <input
      type="text"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      placeholder="Search events by title or location..."
      class="search-input"
    />
  </div>
</template>

<script setup>
defineProps({
  modelValue: {
    type: String,
    default: ''
  }
});
defineEmits(['update:modelValue']);
</script>

<style scoped>
.search-bar {
  width: 100%;
  margin-bottom: 1rem;
}
.search-input {
  width: 100%;
  padding: 0.75rem 1rem;
  border: 1.5px solid #e0e0e0;
  border-radius: 10px;
  font-size: 1rem;
  font-family: inherit;
  outline: none;
  background: #ffffff;
  color: #333;
  transition: border-color 0.2s, box-shadow 0.2s;
}
.search-input:focus {
  border-color: #6644ff;
  box-shadow: 0 0 0 3px rgba(102, 68, 255, 0.12);
}
</style>
