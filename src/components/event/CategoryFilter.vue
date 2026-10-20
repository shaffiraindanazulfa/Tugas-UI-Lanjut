<template>
  <div class="category-filter">
    <button
      v-for="cat in categories"
      :key="cat"
      :class="['filter-btn', { active: modelValue === cat }]"
      @click="$emit('update:modelValue', cat)"
    >
      {{ cat }}
    </button>
  </div>
</template>

<script setup>
defineProps({
  categories: {
    type: Array,
    required: true
  },
  modelValue: {
    type: String,
    default: 'All'
  }
});
defineEmits(['update:modelValue']);
</script>

<style scoped>
.category-filter {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 2rem;
}

.filter-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.45rem 1.1rem;
  border-radius: 50px;
  border: 1.5px solid #e0e0e0;
  background: #ffffff;
  color: #666;
  font-size: 0.9rem;
  font-weight: 500;
  font-family: inherit;
  cursor: pointer;
  white-space: nowrap;
  transition: all 0.2s ease;
  line-height: 1;
}

.filter-btn:hover {
  border-color: #6644ff;
  color: #6644ff;
  background: rgba(102, 68, 255, 0.05);
}

.filter-btn.active {
  background: #6644ff;
  color: #ffffff;
  border-color: #6644ff;
  box-shadow: 0 4px 12px rgba(102, 68, 255, 0.25);
}
</style>
