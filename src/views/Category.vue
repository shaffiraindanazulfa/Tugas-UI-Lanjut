<template>
  <div class="category-page fade-in">
    <h2 class="section-title">Explore by Category</h2>

    <div class="category-grid">
      <div
        class="category-card"
        v-for="(cat, index) in categories"
        :key="index"
      >
        <div class="cat-icon">{{ cat.icon }}</div>
        <h3>{{ cat.name }}</h3>
        <p>{{ cat.count }} Events</p>
      </div>
    </div>
  </div>
</template>

<script setup>
const categories = [
  { name: 'Music & Concerts', icon: '🎵', count: 24 },
  { name: 'Technology',       icon: '💻', count: 18 },
  { name: 'Art & Design',     icon: '🎨', count: 12 },
  { name: 'Business',         icon: '💼', count: 30 },
  { name: 'Health & Wellness',icon: '🏃', count: 15 },
  { name: 'Food & Drink',     icon: '🍽️', count: 22 }
]
</script>

<style scoped>
.category-page {
  animation: fadeIn 0.4s ease;
}

.section-title {
  font-size: 1.8rem;
  color: #1c1948;
  margin-bottom: 2rem;
  font-weight: 700;
}

.category-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1.5rem;
}

.category-card {
  background: white;
  padding: 2rem;
  border-radius: 16px;
  text-align: center;
  border: 1px solid #ebebeb;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  transition: all 0.3s ease;
  cursor: pointer;
}

.category-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
  border-color: #6644ff;
}

.cat-icon {
  font-size: 2.8rem;
  margin-bottom: 1rem;
}

.category-card h3 {
  color: #1c1948;
  font-size: 1rem;
  font-weight: 600;
  margin-bottom: 0.4rem;
}

.category-card p {
  color: #888;
  font-size: 0.88rem;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(8px); }
  to   { opacity: 1; transform: translateY(0); }
}
</style>