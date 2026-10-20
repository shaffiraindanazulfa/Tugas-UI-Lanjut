<script setup>
import AppCard from '@/components/ui/AppCard.vue';

defineProps({
  event: {
    type: Object,
    required: true
  }
});
defineEmits(['view-detail']);
</script>

<template>
  <AppCard class="event-catalog-card">
    <!-- Top row: Date & Category -->
    <div class="card-top">
      <span class="date-badge">{{ event.date }}</span>
      <span class="category-name">{{ event.cat }}</span>
    </div>

    <!-- Title -->
    <h3 class="event-title">{{ event.title }}</h3>

    <!-- Location -->
    <div v-if="event.loc" class="event-location">
      <span class="loc-icon">📍</span>
      <span>{{ event.loc }}</span>
    </div>

    <!-- Description -->
    <p class="event-description">{{ event.desc }}</p>

    <!-- Action: View Details -->
    <div class="card-bottom">
      <button class="btn-view-details" @click="$emit('view-detail', event.id)">
        View Details &rarr;
      </button>
    </div>
  </AppCard>
</template>

<style scoped>
.event-catalog-card {
  padding: 1.85rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  height: 100%;
  box-sizing: border-box;
  background: #ffffff;
  border-radius: 18px;
  border: 1px solid #e2e8f0;
  transition: transform 0.25s ease, box-shadow 0.25s ease, border-color 0.25s ease;
}

.event-catalog-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 28px rgba(0, 0, 0, 0.06);
  border-color: #cbd5e1;
}

.card-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.date-badge {
  background: #ede9fe;
  color: #6d28d9;
  padding: 0.35rem 0.75rem;
  border-radius: 6px;
  font-weight: 600;
  font-size: 0.8rem;
  letter-spacing: -0.01em;
}

.category-name {
  color: #64748b;
  font-size: 0.8rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.06em;
}

.event-title {
  font-size: 1.45rem;
  font-weight: 800;
  color: #1c1948;
  margin: 0;
  line-height: 1.25;
  letter-spacing: -0.01em;
}

.event-location {
  color: #64748b;
  font-size: 0.92rem;
  font-weight: 500;
  display: flex;
  align-items: center;
  gap: 0.4rem;
}

.event-description {
  color: #475569;
  font-size: 0.95rem;
  line-height: 1.65;
  margin: 0;
  display: -webkit-box;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  overflow: hidden;
  flex-grow: 1;
}

.card-bottom {
  margin-top: auto;
  padding-top: 1.15rem;
  border-top: 1px solid #f1f5f9;
}

.btn-view-details {
  background: none;
  border: none;
  color: #5b36f5;
  font-weight: 700;
  font-size: 0.92rem;
  cursor: pointer;
  padding: 0;
  display: inline-flex;
  align-items: center;
  gap: 0.35rem;
  font-family: inherit;
  transition: all 0.2s ease;
}

.btn-view-details:hover {
  color: #4a27e0;
  transform: translateX(3px);
}
</style>
