<script setup>
defineProps({
  isOpen: { type: Boolean, default: false },
  title: { type: String, default: 'Action Confirmation' },
  message: { type: String, default: 'Are you sure you want to proceed?' },
  confirmText: { type: String, default: 'Yes, Proceed' },
  cancelText: { type: String, default: 'Cancel' }
});

defineEmits(['confirm', 'close']);
</script>

<template>
  <Transition name="modal-fade">
    <div v-if="isOpen" class="modal-overlay" @click.self="$emit('close')">
      <div class="modal-card">
        <h3 class="modal-title">
          <span class="modal-icon">⚠️</span>
          {{ title }}
        </h3>
        <p class="modal-message">{{ message }}</p>
        <div class="modal-footer">
          <button class="btn-cancel" @click="$emit('close')">
            {{ cancelText }}
          </button>
          <button class="btn-confirm" @click="$emit('confirm')">
            {{ confirmText }}
          </button>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0; left: 0;
  width: 100vw; height: 100vh;
  background-color: rgba(15, 23, 42, 0.45);
  backdrop-filter: blur(8px);
  display: flex; align-items: center; justify-content: center;
  z-index: 9999;
}

.modal-card {
  background: #ffffff;
  border-radius: 16px;
  padding: 28px;
  max-width: 460px;
  width: 90%;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.12);
  text-align: left;
}

.modal-title {
  font-size: 1.25rem;
  font-weight: 700;
  color: #1c1948;
  margin-bottom: 0.75rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.modal-icon {
  font-size: 1.2rem;
}

.modal-message {
  font-size: 0.95rem;
  color: #64748b;
  margin-bottom: 1.75rem;
  line-height: 1.6;
}

.modal-footer {
  display: flex;
  justify-content: flex-end;
  gap: 0.75rem;
}

.btn-cancel {
  padding: 0.65rem 1.25rem;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
  background: white;
  color: #475569;
  font-weight: 600;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.2s;
  font-family: inherit;
}

.btn-cancel:hover {
  background: #f8fafc;
  color: #1e293b;
}

.btn-confirm {
  padding: 0.65rem 1.35rem;
  border-radius: 8px;
  border: none;
  background: #dc2626;
  color: white;
  font-weight: 600;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.2s;
  font-family: inherit;
}

.btn-confirm:hover {
  background: #b91c1c;
}

.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.25s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}
</style>
