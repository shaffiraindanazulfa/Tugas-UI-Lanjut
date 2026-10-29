<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()

const breadcrumbs = computed(() => {
  const matched = route.matched

  let crumbs = matched
    .map((m) => {
      let path = m.path

      if (path.includes(':')) {
        path = route.path
      }

      return {
        name: m.name,
        path: path,
        meta: m.meta
      }
    })
    .filter((m) => m.meta && m.meta.breadcrumb)

  if (route.name === 'event-detail') {
    crumbs.splice(crumbs.length - 1, 0, {
      path: '/browse/events',
      meta: { breadcrumb: 'Event List' }
    })
  }

  // Prepend Beranda if it's not already the first item
  if (crumbs.length === 0 || crumbs[0].meta.breadcrumb !== 'Home') {
    crumbs.unshift({
      path: '/',
      meta: { breadcrumb: 'Home' }
    })
  }

  return crumbs
})
</script>

<template>
  <nav class="breadcrumb" v-if="breadcrumbs.length > 0">
    <ul>
      <li v-for="(crumb, index) in breadcrumbs" :key="index">
        <span v-if="index > 0" class="separator">/</span>

        <router-link
          v-if="index < breadcrumbs.length - 1"
          :to="crumb.path"
        >
          {{ crumb.meta.breadcrumb }}
        </router-link>

        <span v-else class="active-crumb">
          {{ crumb.meta.breadcrumb }}
        </span>
      </li>
    </ul>
  </nav>
</template>

<style scoped>
.breadcrumb {
  margin-bottom: 1.5rem;
  padding: 0.75rem 1rem;
  background: #fff;
  border: 1px solid var(--border, #ebebeb);
  border-radius: 10px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.04);
}

.breadcrumb ul {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  padding: 0;
  margin: 0;
  align-items: center;
  gap: 0;
}

.breadcrumb li {
  display: flex;
  align-items: center;
}

.breadcrumb a {
  text-decoration: none;
  color: var(--primary, #6644ff);
  font-size: 0.88rem;
  font-weight: 500;
  transition: color 0.2s;
}

.breadcrumb a:hover {
  color: var(--primary-hover, #5533ee);
  text-decoration: underline;
}

.separator {
  color: #bbb;
  margin: 0 0.5rem;
  font-size: 0.85rem;
}

.active-crumb {
  color: #555;
  font-size: 0.88rem;
  font-weight: 600;
}
</style>