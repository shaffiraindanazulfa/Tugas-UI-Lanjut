<script setup>
import { ref, computed } from 'vue';
import { useEvents } from '@/composables/useEvents';
import EventCard from '@/components/event/EventCard.vue';
import SearchBar from '@/components/event/SearchBar.vue';
import CategoryFilter from '@/components/event/CategoryFilter.vue';
import AppButton from '@/components/ui/AppButton.vue';

const { activeEvents } = useEvents();

const searchQuery = ref('');
const selectedCategory = ref('All');
const categories = ['All', 'Workshop', 'Meetup', 'Competition', 'Seminar', 'Conference'];

const filteredEvents = computed(() => {
  return activeEvents.value.filter(event => {
    const matchSearch =
      event.title.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      event.loc.toLowerCase().includes(searchQuery.value.toLowerCase());
    const matchCategory =
      selectedCategory.value === 'All' || event.cat === selectedCategory.value;

    return matchSearch && matchCategory;
  });
});
</script>

<template>
  <div class="event-list-page">
    <div class="header-section">
      <h1 class="section-title">Upcoming Events</h1>
      <p class="section-desc">
        Discover workshops, seminars, tech meetups, and competitions near you.
      </p>
    </div>

    <SearchBar v-model="searchQuery" />
    <CategoryFilter :categories="categories" v-model="selectedCategory" />

    <!-- 1. Grid ketika ada event yang cocok -->
    <div v-if="filteredEvents.length > 0" class="event-grid">
      <EventCard
        v-for="event in filteredEvents"
        :key="event.id"
        :event="event"
        @view-detail="$router.push(`/browse/events/${event.id}`)"
      />
    </div>

    <!-- 2. Empty state saat pencarian/filter tidak ditemukan -->
    <div v-else-if="activeEvents.length > 0" class="search-empty-state">
      <p class="search-empty-text">No events found matching your criteria.</p>
    </div>

    <!-- 3. Empty state saat belum ada event sama sekali di sistem -->
    <div v-else class="empty-state-card">
      <div class="empty-illustration">🗓️</div>
      <h3 class="empty-title">No Events Available Yet</h3>
      <p class="empty-description">
        There are currently no active events scheduled. Are you an organizer?
        Publish and manage your first event today through the Dashboard!
      </p>
      <AppButton variant="primary" @click="$router.push('/dashboard/events')">
        Go to Organizer Dashboard &rarr;
      </AppButton>
    </div>
  </div>
</template>

<style scoped>
.event-list-page {
  animation: fadeIn 0.4s ease;
}

.header-section {
  margin-bottom: 2rem;
}

.section-title {
  font-size: 2.5rem;
  font-weight: 800;
  color: #1c1948;
  margin-bottom: 0.5rem;
  letter-spacing: -0.02em;
}

.section-desc {
  color: #64748b;
  font-size: 1.1rem;
}

.event-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 2rem;
}

/* Empty state untuk pencarian */
.search-empty-state {
  text-align: center;
  padding: 4.5rem 1rem;
}

.search-empty-text {
  color: #64748b;
  font-size: 1.05rem;
  font-weight: 500;
  margin: 0;
}

/* Empty state ketika sistem belum ada event sama sekali */
.empty-state-card {
  text-align: center;
  padding: 4.5rem 2rem;
  background: white;
  border-radius: 20px;
  border: 1px dashed #cbd5e1;
  max-width: 680px;
  margin: 1.5rem auto 3rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.02);
}

.empty-illustration {
  font-size: 3.5rem;
  margin-bottom: 1.5rem;
  line-height: 1;
}

.empty-title {
  font-size: 1.65rem;
  font-weight: 800;
  color: #1c1948;
  margin-bottom: 0.75rem;
  letter-spacing: -0.01em;
}

.empty-description {
  color: #64748b;
  font-size: 1rem;
  max-width: 520px;
  margin: 0 auto 2rem;
  line-height: 1.65;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(8px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>