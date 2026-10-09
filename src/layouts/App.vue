<script setup>
import { RouterView } from 'vue-router'
import Navbar from '@/components/app/Navbar.vue'
import Breadcrumb from '@/components/app/Breadcrumb.vue'
</script>

<template>
  <div class="app-layout">
    <Navbar />
    <main class="main-content">
      <div class="main-inner">
        <!-- <Breadcrumb /> -->
        <RouterView />
      </div>
    </main>
  </div>
</template>

<style scoped>
.app-layout {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: #f5f5f7;
}

.main-content {
  flex: 1;
  padding: 2rem 0 4rem;
}

.main-inner {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

@media (max-width: 768px) {
  .main-inner {
    padding: 0 1rem;
  }
}
</style>
