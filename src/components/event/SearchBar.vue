<script setup>
defineProps({
  modelValue: String,
  placeholder: {
    type: String,
    default: 'Search events by title or location...'
  }
});
defineEmits(['update:modelValue']);
</script>

<template>
  <div class="search-bar-wrapper">
    <input
      :value="modelValue"
      :placeholder="placeholder"
      class="search-input"
      @input="$emit('update:modelValue', $event.target.value)"
    />
  </div>
</template>

<style scoped>
.search-bar-wrapper {
  width: 100%;
  max-width: 460px;
  margin-bottom: 1.25rem;
}

.search-input {
  width: 100%;
  padding: 0.75rem 1.2rem;
  border: 1px solid #e2e8f0;
  border-radius: 10px;
  font-size: 0.95rem;
  font-family: inherit;
  color: #1e293b;
  background: #ffffff;
  outline: none;
  transition: all 0.2s ease;
}

.search-input::placeholder {
  color: #94a3b8;
}

.search-input:focus {
  border-color: #6644ff;
  box-shadow: 0 0 0 3px rgba(102, 68, 255, 0.12);
}
</style>
