<script setup>
import { computed } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import { useEvents } from '@/composables/useEvents';
import AppButton from '@/components/ui/AppButton.vue';

const route = useRoute();
const router = useRouter();
const { getEventById } = useEvents();

const event = computed(() => getEventById(route.params.id));
</script>

<template>
  <div v-if="event" class="detail-container">
    <div class="detail-header">
      <span class="category-pill">{{ event.cat }}</span>
      <span :class="['status-tag', event.done ? 'status-ended' : 'status-live']">
        {{ event.done ? 'Event Concluded' : 'Open Registration' }}
      </span>
      <h1 class="event-title">{{ event.title }}</h1>
      <p class="event-meta">🗓 {{ event.date }} &nbsp;|&nbsp; 📍 {{ event.loc }}</p>
    </div>

    <div class="detail-body">
      <h3>About This Event</h3>
      <p class="description-text">{{ event.desc }}</p>
    </div>

    <div class="action-footer">
      <AppButton v-if="!event.done" variant="primary">
        Register Now (${{ event.price }})
      </AppButton>
      <AppButton v-else variant="secondary" class="btn-disabled" disabled>
        Registration Closed
      </AppButton>
    </div>
  </div>

  <div v-else class="not-found-card">
    <div class="not-found-icon">🔍</div>
    <h2>Event Not Found</h2>
    <p>The event you are looking for does not exist or has been removed.</p>
    <AppButton variant="primary" @click="router.push('/browse/events')">
      Back to Event Catalog
    </AppButton>
  </div>
</template>

<style scoped>
.detail-container {
  max-width: 800px;
  margin: 0 auto;
  background: white;
  border-radius: 20px;
  border: 1px solid #f0f0f0;
  padding: 3rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.02);
}

.detail-header {
  margin-bottom: 2.5rem;
}

.category-pill {
  display: inline-block;
  background: rgba(102, 68, 255, 0.1);
  color: #6644ff;
  padding: 0.35rem 0.9rem;
  border-radius: 50px;
  font-weight: 600;
  font-size: 0.85rem;
  margin-right: 0.6rem;
  margin-bottom: 1rem;
}

.status-tag {
  display: inline-block;
  padding: 0.35rem 0.9rem;
  border-radius: 50px;
  font-weight: 600;
  font-size: 0.85rem;
  margin-bottom: 1rem;
}

.status-live {
  background: #e6f7ed;
  color: #10b981;
}

.status-ended {
  background: #f3f4f6;
  color: #9ca3af;
}

.event-title {
  color: #1c1948;
  font-size: 2.4rem;
  font-weight: 800;
  margin-bottom: 1rem;
  line-height: 1.25;
}

.event-meta {
  color: #666;
  font-size: 1rem;
  font-weight: 500;
}

.detail-body {
  border-top: 1px solid #f0f0f0;
  padding-top: 2rem;
  margin-bottom: 2.5rem;
}

.detail-body h3 {
  color: #1c1948;
  font-size: 1.3rem;
  margin-bottom: 1rem;
}

.description-text {
  color: #444;
  line-height: 1.8;
  font-size: 1.05rem;
}

.action-footer {
  border-top: 1px solid #f0f0f0;
  padding-top: 2rem;
  display: flex;
  gap: 1rem;
}

.btn-disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.not-found-card {
  text-align: center;
  padding: 4rem 2rem;
  background: white;
  border-radius: 16px;
  border: 1px solid #f0f0f0;
  max-width: 600px;
  margin: 2rem auto;
}

.not-found-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.not-found-card h2 {
  color: #1c1948;
  margin-bottom: 0.5rem;
}

.not-found-card p {
  color: #666;
  margin-bottom: 1.5rem;
}
</style>