<script setup>
import { RouterView, useRoute } from 'vue-router';

const route = useRoute();
</script>

<template>
  <div class="dashboard-layout">
    <aside class="dashboard-sidebar">
      <div class="sidebar-top">
        <div class="sidebar-brand">
          Gatherly Organizer
        </div>

        <nav class="rail-nav">
          <router-link
            to="/dashboard"
            class="rail-link"
            exact-active-class="active"
          >
            <span class="rail-label">Dashboard</span>
          </router-link>

          <router-link
            to="/dashboard/events"
            class="rail-link"
            active-class="active"
          >
            <span class="rail-label">Event Management</span>
          </router-link>
        </nav>
      </div>

      <div class="sidebar-bottom">
        <router-link to="/" class="back-link">
          &larr; Back to Public
        </router-link>
      </div>
    </aside>

    <div class="dashboard-main-wrapper">
      <header class="dashboard-topbar">
        <span class="topbar-title">Dashboard</span>
        <span class="topbar-user">Admin</span>
      </header>

      <main class="dashboard-content">
        <RouterView />
      </main>
    </div>
  </div>
</template>

<style scoped>
.dashboard-layout {
  display: flex;
  min-height: 100vh;
  background: #f8fafc;
  font-family: inherit;
}

.dashboard-sidebar {
  width: 240px;
  background: #14113c;
  padding: 1.5rem 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  flex-shrink: 0;
  border-right: 1px solid rgba(255, 255, 255, 0.05);
}

.sidebar-brand {
  color: #ffffff;
  font-size: 1.15rem;
  font-weight: 700;
  padding: 0.5rem 0.75rem 1.5rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  margin-bottom: 1rem;
  letter-spacing: -0.01em;
}

.rail-nav {
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
}

.rail-link {
  display: flex;
  align-items: center;
  padding: 0.75rem 1rem;
  border-radius: 8px;
  color: #94a3b8;
  text-decoration: none;
  font-size: 0.95rem;
  font-weight: 500;
  transition: all 0.2s ease;
}

.rail-link:hover {
  background: rgba(255, 255, 255, 0.06);
  color: #ffffff;
  text-decoration: none;
}

.rail-link.active {
  background: rgba(255, 255, 255, 0.12);
  color: #ffffff;
  font-weight: 600;
}

.sidebar-bottom {
  padding: 1rem 0.5rem 0.5rem;
  border-top: 1px solid rgba(255, 255, 255, 0.08);
}

.back-link {
  color: #94a3b8;
  font-size: 0.9rem;
  font-weight: 500;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  transition: color 0.2s;
}

.back-link:hover {
  color: #ffffff;
  text-decoration: none;
}

.dashboard-main-wrapper {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  overflow-x: hidden;
}

.dashboard-topbar {
  background: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  padding: 1rem 3rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.topbar-title {
  font-size: 1.25rem;
  font-weight: 800;
  color: #1c1948;
}

.topbar-user {
  font-size: 0.95rem;
  font-weight: 600;
  color: #64748b;
}

.dashboard-content {
  flex: 1;
  padding: 2.5rem 3rem;
  overflow-y: auto;
}

@media (max-width: 900px) {
  .dashboard-layout {
    flex-direction: column;
  }
  .dashboard-sidebar {
    width: 100%;
    flex-direction: row;
    align-items: center;
    padding: 1rem;
  }
  .sidebar-brand {
    border-bottom: none;
    padding: 0;
    margin-bottom: 0;
  }
  .rail-nav {
    flex-direction: row;
  }
  .sidebar-bottom {
    display: none;
  }
  .dashboard-content {
    padding: 1.5rem;
  }
}
</style>
