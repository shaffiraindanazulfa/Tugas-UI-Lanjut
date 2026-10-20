<template>
  <div class="event-list-page">
    <div class="header-section">
      <h2 class="section-title">Upcoming Events</h2>
      <p class="section-desc">
        Discover the latest gatherings and activities happening near you
      </p>

      <!-- Search Bar -->
      <SearchBar v-model="searchQuery" />

      <!-- Category Filter -->
      <CategoryFilter v-model="activeCategory" :categories="categories" />
    </div>

    <!-- Event Grid -->
    <div class="event-grid" v-if="filteredEvents.length > 0">
      <EventCard
        v-for="event in filteredEvents"
        :key="event.id"
        :event="event"
        @view-detail="(id) => $router.push(`/browse/events/${id}`)"
      />
    </div>

    <!-- Empty State -->
    <div class="empty-state" v-else>
      <p class="empty-icon">🔍</p>
      <p class="empty-title">No events found</p>
      <p class="empty-desc">Try a different keyword or category.</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import SearchBar from '@/components/event/SearchBar.vue';
import CategoryFilter from '@/components/event/CategoryFilter.vue';
import EventCard from '@/components/event/EventCard.vue';

const searchQuery = ref('');
const activeCategory = ref('All');

const categories = ['All', 'Workshop', 'Meetup', 'Competition', 'Seminar', 'Conference'];

const events = ref([
  {
    id: 1,
    title: 'Vue.js Workshop 2026',
    date: 'Oct 11, 2026',
    cat: 'Workshop',
    loc: '📍 Main Auditorium, City Center',
    desc: 'An immersive hands-on workshop covering the latest Vue.js 3 features, Composition API, and best practices for modern frontend development.',
  },
  {
    id: 2,
    title: 'Dev Meetup: UI/UX Trends',
    date: 'Oct 12, 2026',
    cat: 'Meetup',
    loc: '📍 Coworking Space, Downtown',
    desc: 'Join fellow designers and developers to discuss the hottest UI/UX trends and tools shaping the web in 2026.',
  },
  {
    id: 3,
    title: 'National Coding Competition',
    date: 'Oct 13, 2026',
    cat: 'Competition',
    loc: '📍 Tech Hub, East District',
    desc: 'Compete against the brightest minds in a 24-hour coding marathon with exciting prizes and networking opportunities.',
  },
  {
    id: 4,
    title: 'AI in Education Seminar',
    date: 'Oct 14, 2026',
    cat: 'Seminar',
    loc: '📍 University Hall, Campus Area',
    desc: 'Experts share insights on how artificial intelligence is transforming classrooms, learning tools, and educational outcomes worldwide.',
  },
  {
    id: 5,
    title: 'Global Tech Conference 2026',
    date: 'Oct 15, 2026',
    cat: 'Conference',
    loc: '📍 Convention Center, North Wing',
    desc: 'A world-class conference bringing together tech leaders, innovators, and entrepreneurs from across the globe.',
  },
  {
    id: 6,
    title: 'Community Developers Meetup',
    date: 'Oct 16, 2026',
    cat: 'Meetup',
    loc: '📍 Startup Hub, Central Park',
    desc: 'A relaxed evening meetup for local developers to share projects, swap ideas, and build connections in the community.',
  },
]);

const filteredEvents = computed(() => {
  return events.value.filter((event) => {
    const matchesCategory =
      activeCategory.value === 'All' || event.cat === activeCategory.value;
    const matchesSearch =
      event.title.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      event.loc.toLowerCase().includes(searchQuery.value.toLowerCase());
    return matchesCategory && matchesSearch;
  });
});
</script>

<style scoped>
.event-list-page {
  padding: 3rem 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.header-section {
  margin-bottom: 3rem;
}

.section-title {
  font-size: 2.2rem;
  color: #1c1948;
  margin-bottom: 0.5rem;
}

.section-desc {
  color: #666;
  font-size: 1.1rem;
  margin-bottom: 1.5rem;
}

.event-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 2rem;
}

/* Empty State */
.empty-state {
  text-align: center;
  padding: 5rem 2rem;
  color: #888;
}

.empty-icon {
  font-size: 3rem;
  margin-bottom: 0.75rem;
}

.empty-title {
  font-size: 1.4rem;
  font-weight: 600;
  color: #444;
  margin-bottom: 0.4rem;
}

.empty-desc {
  font-size: 1rem;
  color: #999;
}
</style>