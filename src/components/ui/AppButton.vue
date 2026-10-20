<script setup>
defineProps({
  variant: {
    type: String,
    default: 'primary',
  },
});
defineEmits(['click']);
</script>

<template>
  <button :class="['app-button', `btn-${variant}`]"
    @click="$emit('click')" >
    <slot />
  </button>
</template>

<style scoped>
.app-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 0.8rem 1.6rem;
  border-radius: 12px;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  border: none;
  font-family: inherit;
  transition: all 0.25s ease;
}

.btn-primary {
  background: #6644ff;
  color: white;
  box-shadow: 0 4px 14px rgba(102, 68, 255, 0.3);
}

.btn-primary:hover {
  background: #5533ee;
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(102, 68, 255, 0.4);
}

.btn-secondary {
  background: white;
  color: #1c1948;
  border: 1px solid #dedede;
}

.btn-secondary:hover {
  border-color: #6644ff;
  color: #6644ff;
}
</style>
