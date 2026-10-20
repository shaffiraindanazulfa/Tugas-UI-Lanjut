<script setup>
import { useRouter } from 'vue-router';
import { useEvents } from '@/composables/useEvents';

const router = useRouter();
const { events, totalEvents, activeEvents, completedEvents } = useEvents();
</script>

<template>
  <div class="dashboard-page">
    <div class="dashboard-header">
      <div>
        <h1 class="page-title">Organizer Analytics &amp; Overview</h1>
        <p class="page-subtitle">Monitor real-time statistics and track status across all managed events.</p>
      </div>
      <button class="btn-manage-events" @click="router.push('/dashboard/events')">
        💳 Manage Events &rarr;
      </button>
    </div>

    <!-- Metrics Grid -->
    <div class="metrics-grid">
      <div class="metric-card">
        <span class="metric-label">TOTAL EVENTS</span>
        <p class="metric-value text-primary">{{ totalEvents }}</p>
        <span class="metric-sub">All recorded events</span>
      </div>

      <div class="metric-card">
        <span class="metric-label">ACTIVE EVENTS</span>
        <p class="metric-value text-success">{{ activeEvents.length }}</p>
        <span class="metric-sub">Live in public catalog</span>
      </div>

      <div class="metric-card">
        <span class="metric-label">COMPLETED EVENTS</span>
        <p class="metric-value text-muted">{{ completedEvents.length }}</p>
        <span class="metric-sub">Archived completed events</span>
      </div>
    </div>

    <!-- Recent Events Status Overview Card -->
    <div class="overview-card">
      <div class="overview-header">
        <div>
          <h2 class="overview-title">Recent Events Status Overview</h2>
          <p class="overview-subtitle">Master registry of all events managed across the Gatherly platform.</p>
        </div>
        <button class="btn-open-manage" @click="router.push('/dashboard/events')">
          Open Event Management &rarr;
        </button>
      </div>

      <div class="table-container">
        <table class="data-table">
          <thead>
            <tr>
              <th>EVENT TITLE</th>
              <th>CATEGORY</th>
              <th>DATE &amp; LOCATION</th>
              <th>QUOTA</th>
              <th>STATUS</th>
            </tr>
          </thead>
          <tbody>
            <tr v-if="events.length === 0">
              <td colspan="5" class="empty-table-cell">
                <div class="empty-state-inner">
                  <div class="empty-icon">📊</div>
                  <p class="empty-text">No events recorded in the system yet.</p>
                  <button class="btn-create-first" @click="router.push('/dashboard/events')">
                    Create First Event
                  </button>
                </div>
              </td>
            </tr>

            <tr v-for="item in events" :key="item.id" v-else>
              <td class="font-semibold">{{ item.title }}</td>
              <td>
                <span class="cat-pill">{{ item.cat }}</span>
              </td>
              <td class="meta-cell">
                {{ item.date }}<span v-if="item.loc"> • 📍 {{ item.loc }}</span>
              </td>
              <td class="quota-cell">{{ item.quota ? item.quota + ' Pax' : '-' }}</td>
              <td>
                <span :class="['status-badge', item.done ? 'status-completed' : 'status-active']">
                  {{ item.done ? 'Completed' : 'Active' }}
                </span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<style scoped>
.dashboard-page {
  animation: fadeIn 0.4s ease;
}

.dashboard-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 2.5rem;
  gap: 1.5rem;
}

.page-title {
  font-size: 2.2rem;
  font-weight: 800;
  color: #1c1948;
  margin-bottom: 0.4rem;
  letter-spacing: -0.02em;
}

.page-subtitle {
  color: #64748b;
  font-size: 1rem;
}

.btn-manage-events {
  background: #5b36f5;
  color: white;
  border: none;
  padding: 0.8rem 1.6rem;
  border-radius: 12px;
  font-weight: 600;
  font-size: 0.95rem;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.2s ease;
  box-shadow: 0 4px 14px rgba(91, 54, 245, 0.3);
  font-family: inherit;
  white-space: nowrap;
}

.btn-manage-events:hover {
  background: #4a27e0;
  transform: translateY(-1px);
  box-shadow: 0 6px 20px rgba(91, 54, 245, 0.4);
}

.metrics-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
  margin-bottom: 2.5rem;
}

.metric-card {
  background: #ffffff;
  border-radius: 16px;
  padding: 1.75rem 2rem;
  border: 1px solid #e2e8f0;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.02);
  display: flex;
  flex-direction: column;
}

.metric-label {
  font-size: 0.8rem;
  font-weight: 700;
  color: #64748b;
  letter-spacing: 0.05em;
  text-transform: uppercase;
}

.metric-value {
  font-size: 2.75rem;
  font-weight: 800;
  line-height: 1.1;
  margin: 0.6rem 0;
  letter-spacing: -0.02em;
}

.text-primary {
  color: #5b36f5;
}

.text-success {
  color: #10b981;
}

.text-muted {
  color: #64748b;
}

.metric-sub {
  font-size: 0.85rem;
  color: #94a3b8;
}

.overview-card {
  background: #ffffff;
  border-radius: 16px;
  padding: 2rem;
  border: 1px solid #e2e8f0;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.02);
}

.overview-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 1.75rem;
  gap: 1rem;
}

.overview-title {
  font-size: 1.25rem;
  font-weight: 800;
  color: #1c1948;
  margin-bottom: 0.35rem;
  letter-spacing: -0.01em;
}

.overview-subtitle {
  font-size: 0.9rem;
  color: #64748b;
}

.btn-open-manage {
  background: none;
  border: none;
  color: #5b36f5;
  font-weight: 600;
  font-size: 0.92rem;
  cursor: pointer;
  transition: color 0.2s;
  font-family: inherit;
  white-space: nowrap;
}

.btn-open-manage:hover {
  color: #4a27e0;
  text-decoration: underline;
}

.table-container {
  overflow-x: auto;
}

.data-table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
}

.data-table th {
  padding: 0.85rem 1rem;
  font-size: 0.78rem;
  font-weight: 700;
  color: #64748b;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  border-bottom: 1px solid #f1f5f9;
}

.data-table td {
  padding: 1.15rem 1rem;
  border-bottom: 1px solid #f8fafc;
  font-size: 0.95rem;
  color: #334155;
}

.font-semibold {
  font-weight: 600;
  color: #1e293b;
}

.cat-pill {
  background: #ede9fe;
  color: #7c3aed;
  font-weight: 700;
  font-size: 0.72rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  padding: 0.25rem 0.65rem;
  border-radius: 50px;
  display: inline-block;
}

.meta-cell {
  color: #475569;
}

.quota-cell {
  font-weight: 500;
  color: #334155;
}

.empty-table-cell {
  padding: 3.5rem 1rem;
  text-align: center;
  border: none;
}

.empty-state-inner {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.empty-icon {
  font-size: 2.75rem;
  margin-bottom: 1rem;
  line-height: 1;
}

.empty-text {
  font-size: 0.95rem;
  color: #64748b;
  margin-bottom: 1.25rem;
}

.btn-create-first {
  background: #5b36f5;
  color: white;
  border: none;
  padding: 0.75rem 1.6rem;
  border-radius: 10px;
  font-weight: 600;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 4px 14px rgba(91, 54, 245, 0.3);
  font-family: inherit;
}

.btn-create-first:hover {
  background: #4a27e0;
  transform: translateY(-1px);
}

.status-badge {
  display: inline-block;
  padding: 0.25rem 0.75rem;
  border-radius: 50px;
  font-size: 0.8rem;
  font-weight: 600;
}

.status-active {
  background: #dcfce7;
  color: #15803d;
}

.status-completed {
  background: #f1f5f9;
  color: #64748b;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (max-width: 900px) {
  .metrics-grid {
    grid-template-columns: 1fr;
  }
  .dashboard-header {
    flex-direction: column;
  }
}
</style>
