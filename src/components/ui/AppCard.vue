<template>
  <div class="app-card">
    <slot />
  </div>
</template>

<style scoped>
.app-card {
  background: #ffffff;
  border-radius: 18px;
  border: 1px solid #e2e8f0;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.02);
  transition: all 0.25s ease;
  overflow: hidden;
}
</style>
