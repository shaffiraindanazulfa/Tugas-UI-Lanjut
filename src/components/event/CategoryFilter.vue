<script setup>
defineProps({
  categories: {
    type: Array,
    default: () => ['All', 'Workshop', 'Meetup', 'Competition', 'Seminar', 'Conference']
  },
  modelValue: {
    type: String,
    default: 'All'
  }
});
defineEmits(['update:modelValue']);
</script>

<template>
  <div class="category-filter-list">
    <button
      v-for="cat in categories"
      :key="cat"
      type="button"
      :class="['category-pill', { active: modelValue === cat }]"
      @click="$emit('update:modelValue', cat)"
    >
      {{ cat }}
    </button>
  </div>
</template>

<style scoped>
.category-filter-list {
  display: flex;
  flex-wrap: wrap;
  gap: 0.6rem;
  margin-bottom: 2.5rem;
}

.category-pill {
  padding: 0.45rem 1.15rem;
  border-radius: 50px;
  font-size: 0.88rem;
  font-weight: 500;
  cursor: pointer;
  border: 1px solid #e2e8f0;
  background: #ffffff;
  color: #475569;
  transition: all 0.2s ease;
  font-family: inherit;
}

.category-pill:hover {
  border-color: #cbd5e1;
  color: #1e293b;
}

.category-pill.active {
  background: #6644ff;
  border-color: #6644ff;
  color: #ffffff;
  font-weight: 600;
  box-shadow: 0 2px 10px rgba(102, 68, 255, 0.25);
}
</style>
