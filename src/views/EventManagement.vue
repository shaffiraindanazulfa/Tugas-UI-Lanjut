<script setup>
import { ref, reactive } from 'vue';
import { useEvents } from '@/composables/useEvents';
import AppModal from '@/components/ui/AppModal.vue';

const { activeEvents, completedEvents, createEvent, completeEvent, deleteEvent } = useEvents();

const currentTab = ref('active');
const isModalOpen = ref(false);
const modalAction = ref('complete');
const selectedEvent = ref(null);

const form = reactive({
  title: '',
  cat: 'Workshop',
  date: '',
  loc: '',
  quota: 100,
  price: 'Free',
  desc: ''
});
const errorMessage = ref('');
const successMessage = ref('');

const formatEventDate = (datetimeLocal) => {
  if (!datetimeLocal) return '';
  const d = new Date(datetimeLocal);
  return d.toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' }) +
    ' • ' + d.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' });
};

const handleSubmitEvent = () => {
  if (!form.title.trim() || !form.date || !form.loc.trim() || !form.desc.trim()) {
    errorMessage.value = 'Please fill in all required fields before publishing the event!';
    return;
  }

  createEvent({
    title: form.title.trim(),
    cat: form.cat,
    date: formatEventDate(form.date),
    loc: form.loc.trim(),
    quota: Number(form.quota),
    price: form.price,
    desc: form.desc.trim()
  });

  errorMessage.value = '';
  successMessage.value = 'Event published successfully and is now live!';
  Object.assign(form, { title: '', cat: 'Workshop', date: '', loc: '', quota: 100, price: 'Free', desc: '' });

  setTimeout(() => {
    successMessage.value = '';
    currentTab.value = 'active';
  }, 1200);
};

const handleOpenCompleteModal = (item) => {
  selectedEvent.value = item;
  modalAction.value = 'complete';
  isModalOpen.value = true;
};

const handleOpenDeleteModal = (item) => {
  selectedEvent.value = item;
  modalAction.value = 'delete';
  isModalOpen.value = true;
};

const handleConfirmModal = () => {
  if (!selectedEvent.value) return;
  if (modalAction.value === 'complete') {
    completeEvent(selectedEvent.value.id);
    currentTab.value = 'completed'; // Otomatis beralih ke tab Completed Events
  } else if (modalAction.value === 'delete') {
    deleteEvent(selectedEvent.value.id);
  }
  isModalOpen.value = false;
  selectedEvent.value = null;
};
</script>

<template>
  <div class="event-management-page">
    <!-- Page Header -->
    <div class="page-header">
      <h1 class="header-title">Event Management Control</h1>
      <p class="header-subtitle">
        Manage event lifecycle, publish new events, and archive completed ones.
      </p>
    </div>

    <!-- Tabs Nav -->
    <div class="tabs-nav">
      <button
        :class="['tab-btn', { active: currentTab === 'active' }]"
        @click="currentTab = 'active'"
      >
        ⚡ Active Events ({{ activeEvents.length }})
      </button>

      <button
        :class="['tab-btn', { active: currentTab === 'create' }]"
        @click="currentTab = 'create'"
      >
        ➕ Create Event
      </button>

      <button
        :class="['tab-btn', { active: currentTab === 'completed' }]"
        @click="currentTab = 'completed'"
      >
        ✅ Completed Events ({{ completedEvents.length }})
      </button>
    </div>

    <!-- TAB 1: ACTIVE EVENTS -->
    <section v-if="currentTab === 'active'">
      <div v-if="activeEvents.length > 0" class="events-grid">
        <div
          v-for="item in activeEvents"
          :key="item.id"
          class="event-manage-card"
        >
          <div class="card-badges">
            <span class="cat-pill">{{ item.cat }}</span>
            <span class="status-pill status-active">Active</span>
          </div>

          <h3 class="card-event-title">{{ item.title }}</h3>

          <div class="card-event-meta">
            <span>🗓️ {{ item.date }}</span>
            <span v-if="item.loc">• 📍 {{ item.loc }}</span>
          </div>

          <p class="card-event-desc">{{ item.desc }}</p>

          <div class="card-footer">
            <span class="quota-info">👥 Quota: {{ item.quota || 100 }} seats</span>
            <div class="card-actions">
              <!-- Tombol Complete & Delete sesuai persis screenshot -->
              <button
                class="btn-complete-outline"
                @click="handleOpenCompleteModal(item)"
              >
                <span class="btn-icon">✅</span> Complete Event
              </button>
              <button
                class="btn-delete-outline"
                @click="handleOpenDeleteModal(item)"
              >
                <span class="btn-icon">🗑️</span> Delete
              </button>
            </div>
          </div>
        </div>
      </div>

      <div v-else class="empty-card">
        <div class="empty-icon">⚡</div>
        <h3>No Active Events</h3>
        <p>You don't have any active events currently live. Create your first event now!</p>
        <button class="btn-primary-tab" @click="currentTab = 'create'">
          ➕ Create New Event
        </button>
      </div>
    </section>

    <!-- TAB 2: CREATE EVENT FORM -->
    <section v-else-if="currentTab === 'create'" class="form-card">
      <h2 class="form-title">Create New Event Form</h2>
      <p class="form-subtitle">
        Information entered here will be instantly published to the Gatherly public catalog.
      </p>

      <!-- Error State Alert -->
      <div v-if="errorMessage" class="alert-banner alert-error">
        <span class="alert-icon">⚠️</span>
        <span>{{ errorMessage }}</span>
      </div>

      <!-- Success State Alert -->
      <div v-if="successMessage" class="alert-banner alert-success">
        <span class="alert-icon">🎉</span>
        <span>{{ successMessage }}</span>
      </div>

      <form @submit.prevent="handleSubmitEvent" class="event-form">
        <!-- Event Title -->
        <div class="form-group">
          <label>Event Title *</label>
          <input
            v-model="form.title"
            type="text"
            placeholder="e.g. Modern UI/UX Masterclass"
          />
        </div>

        <!-- Category & Date Row -->
        <div class="form-row">
          <div class="form-group">
            <label>Event Category *</label>
            <select v-model="form.cat">
              <option value="Workshop">Workshop</option>
              <option value="Meetup">Meetup</option>
              <option value="Competition">Competition</option>
              <option value="Seminar">Seminar</option>
              <option value="Conference">Conference</option>
            </select>
          </div>

          <div class="form-group">
            <label>Date &amp; Time *</label>
            <input
              v-model="form.date"
              type="datetime-local"
            />
          </div>
        </div>

        <!-- Location & Quota Row -->
        <div class="form-row">
          <div class="form-group">
            <label>Event Location *</label>
            <input
              v-model="form.loc"
              type="text"
              placeholder="e.g. Innovation Center, Jakarta"
            />
          </div>

          <div class="form-group">
            <label>Attendee Quota</label>
            <input
              v-model="form.quota"
              type="number"
              placeholder="100"
            />
          </div>
        </div>

        <!-- Event Description -->
        <div class="form-group">
          <label>Event Description *</label>
          <textarea
            v-model="form.desc"
            rows="4"
            placeholder="Describe the agenda, speakers, and key takeaways..."
          ></textarea>
        </div>

        <!-- Form Action Buttons -->
        <div class="form-actions">
          <button
            type="button"
            class="btn-cancel"
            @click="currentTab = 'active'"
          >
            Cancel
          </button>
          <button type="submit" class="btn-publish">
            🚀 Publish Event Now
          </button>
        </div>
      </form>
    </section>

    <!-- TAB 3: COMPLETED EVENTS (CROSSED OUT) -->
    <section v-else-if="currentTab === 'completed'">
      <div v-if="completedEvents.length > 0" class="events-grid">
        <div
          v-for="item in completedEvents"
          :key="item.id"
          class="event-manage-card completed-card"
        >
          <div class="card-badges">
            <span class="cat-pill">{{ item.cat }}</span>
            <span class="status-pill status-completed-badge">Completed</span>
          </div>

          <!-- Crossed out title sesuai instruksi gambar -->
          <h3 class="card-event-title crossed-out">{{ item.title }}</h3>

          <div class="card-event-meta">
            <span>🗓️ {{ item.date }}</span>
            <span v-if="item.loc">• 📍 {{ item.loc }}</span>
          </div>

          <p class="card-event-desc">{{ item.desc }}</p>

          <!-- Archived Notice -->
          <div class="archived-notice">
            <span>🔒 This event has ended and is archived from the public catalog.</span>
          </div>

          <div class="card-footer">
            <span></span>
            <button
              class="btn-delete-outline"
              @click="handleOpenDeleteModal(item)"
            >
              <span class="btn-icon">🗑️</span> Delete from Archives
            </button>
          </div>
        </div>
      </div>

      <div v-else class="empty-card">
        <div class="empty-icon">✅</div>
        <h3>No Completed Events</h3>
        <p>No events have been marked as completed yet.</p>
      </div>
    </section>

    <!-- Modal Konfirmasi Sesuai Persis Screenshot -->
    <AppModal
      :is-open="isModalOpen"
      :title="modalAction === 'complete' ? 'Complete Event?' : 'Delete Event Permanently?'"
      :message="modalAction === 'complete'
        ? `Are you sure you want to mark '${selectedEvent?.title}' as completed? Once completed, the status is final and the event will be automatically removed from public listings.`
        : `Are you sure you want to permanently delete '${selectedEvent?.title}'? This action cannot be undone and will permanently remove this event from all records.`"
      :confirm-text="modalAction === 'complete' ? 'Yes, Complete' : 'Yes, Delete Permanently'"
      @confirm="handleConfirmModal"
      @close="isModalOpen = false"
    />
  </div>
</template>

<style scoped>
.event-management-page {
  animation: fadeIn 0.4s ease;
}

/* Header */
.page-header {
  margin-bottom: 2rem;
}

.header-title {
  font-size: 2.2rem;
  font-weight: 800;
  color: #1c1948;
  margin-bottom: 0.4rem;
  letter-spacing: -0.02em;
}

.header-subtitle {
  font-size: 1rem;
  color: #64748b;
}

/* Tabs */
.tabs-nav {
  display: flex;
  gap: 0.75rem;
  margin-bottom: 2rem;
}

.tab-btn {
  padding: 0.75rem 1.4rem;
  border-radius: 10px;
  font-weight: 600;
  font-size: 0.95rem;
  cursor: pointer;
  border: 1px solid #e2e8f0;
  background: white;
  color: #64748b;
  transition: all 0.2s ease;
  font-family: inherit;
  display: inline-flex;
  align-items: center;
  gap: 0.45rem;
}

.tab-btn:hover {
  border-color: #cbd5e1;
  color: #1e293b;
}

.tab-btn.active {
  background: #5b36f5;
  color: white;
  border-color: #5b36f5;
  box-shadow: 0 4px 14px rgba(91, 54, 245, 0.3);
}

/* Cards Grid */
.events-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(440px, 1fr));
  gap: 1.75rem;
}

.event-manage-card {
  background: #ffffff;
  border-radius: 18px;
  border: 1px solid #e2e8f0;
  padding: 2rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.02);
  display: flex;
  flex-direction: column;
  gap: 0.85rem;
}

.card-badges {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cat-pill {
  background: #ede9fe;
  color: #7c3aed;
  font-weight: 700;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  padding: 0.35rem 0.85rem;
  border-radius: 50px;
}

.status-pill {
  padding: 0.3rem 0.85rem;
  border-radius: 50px;
  font-size: 0.8rem;
  font-weight: 600;
}

.status-active {
  background: #ecfdf5;
  color: #059669;
}

.status-completed-badge {
  background: #f1f5f9;
  color: #64748b;
}

.card-event-title {
  font-size: 1.4rem;
  font-weight: 800;
  color: #1e293b;
  margin: 0;
  line-height: 1.3;
}

/* Crossed out title ketika status selesai */
.card-event-title.crossed-out {
  text-decoration: line-through;
  color: #94a3b8;
}

.card-event-meta {
  font-size: 0.92rem;
  color: #64748b;
  font-weight: 500;
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
}

.card-event-desc {
  font-size: 0.95rem;
  color: #475569;
  line-height: 1.65;
  margin: 0;
}

.archived-notice {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 0.75rem 1rem;
  font-size: 0.88rem;
  color: #64748b;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 0.5rem;
  padding-top: 1.25rem;
  border-top: 1px solid #f1f5f9;
}

.quota-info {
  font-size: 0.88rem;
  color: #64748b;
  font-weight: 500;
}

.card-actions {
  display: flex;
  gap: 0.75rem;
}

/* Tombol Complete Event Outline Hijau (sesuai Screenshot 1) */
.btn-complete-outline {
  padding: 0.55rem 1.15rem;
  background: #ffffff;
  border: 1px solid #86efac;
  color: #16a34a;
  border-radius: 8px;
  font-weight: 600;
  font-size: 0.88rem;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 0.45rem;
  transition: all 0.2s ease;
  font-family: inherit;
}

.btn-complete-outline:hover {
  background: #f0fdf4;
  border-color: #4ade80;
}

/* Tombol Delete Outline Merah (sesuai Screenshot 1) */
.btn-delete-outline {
  padding: 0.55rem 1.15rem;
  background: #ffffff;
  border: 1px solid #fecaca;
  color: #dc2626;
  border-radius: 8px;
  font-weight: 600;
  font-size: 0.88rem;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 0.45rem;
  transition: all 0.2s ease;
  font-family: inherit;
}

.btn-delete-outline:hover {
  background: #fef2f2;
  border-color: #f87171;
}

/* Empty Card */
.empty-card {
  text-align: center;
  padding: 4rem 2rem;
  background: white;
  border-radius: 18px;
  border: 1px dashed #cbd5e1;
  max-width: 550px;
  margin: 2rem auto;
}

.empty-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.empty-card h3 {
  font-size: 1.35rem;
  font-weight: 800;
  color: #1c1948;
  margin-bottom: 0.5rem;
}

.empty-card p {
  color: #64748b;
  margin-bottom: 1.5rem;
  font-size: 0.95rem;
}

.btn-primary-tab {
  background: #5b36f5;
  color: white;
  border: none;
  padding: 0.75rem 1.6rem;
  border-radius: 10px;
  font-weight: 600;
  cursor: pointer;
  box-shadow: 0 4px 14px rgba(91, 54, 245, 0.3);
  font-family: inherit;
}

/* Form Card */
.form-card {
  background: #ffffff;
  border-radius: 18px;
  border: 1px solid #e2e8f0;
  padding: 2.25rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.02);
}

.form-title {
  font-size: 1.45rem;
  font-weight: 800;
  color: #1c1948;
  margin-bottom: 0.35rem;
  letter-spacing: -0.01em;
}

.form-subtitle {
  font-size: 0.95rem;
  color: #64748b;
  margin-bottom: 1.75rem;
}

/* Alerts */
.alert-banner {
  border-radius: 10px;
  padding: 0.9rem 1.25rem;
  font-size: 0.92rem;
  font-weight: 500;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  margin-bottom: 1.75rem;
}

.alert-error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #dc2626;
}

.alert-success {
  background: #f0fdf4;
  border: 1px solid #bbf7d0;
  color: #16a34a;
}

.alert-icon {
  font-size: 1.1rem;
}

/* Form Controls */
.event-form {
  display: flex;
  flex-direction: column;
}

.form-group {
  margin-bottom: 1.35rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.25rem;
}

label {
  display: block;
  font-size: 0.88rem;
  font-weight: 600;
  color: #1e293b;
  margin-bottom: 0.5rem;
}

input, select, textarea {
  width: 100%;
  padding: 0.75rem 1rem;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  font-size: 0.92rem;
  color: #1e293b;
  background: white;
  font-family: inherit;
  transition: all 0.2s ease;
  box-sizing: border-box;
}

input::placeholder, textarea::placeholder {
  color: #94a3b8;
}

input:focus, select:focus, textarea:focus {
  border-color: #5b36f5;
  outline: none;
  box-shadow: 0 0 0 3px rgba(91, 54, 245, 0.12);
}

.form-actions {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  gap: 1rem;
  margin-top: 1.5rem;
}

.btn-cancel {
  padding: 0.75rem 1.6rem;
  border: 1px solid #e2e8f0;
  background: white;
  color: #64748b;
  font-weight: 600;
  font-size: 0.92rem;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.2s;
  font-family: inherit;
}

.btn-cancel:hover {
  background: #f8fafc;
  color: #1e293b;
}

.btn-publish {
  padding: 0.75rem 1.8rem;
  background: #5b36f5;
  color: white;
  font-weight: 600;
  font-size: 0.95rem;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  box-shadow: 0 4px 14px rgba(91, 54, 245, 0.3);
  transition: all 0.2s;
  font-family: inherit;
}

.btn-publish:hover {
  background: #4a27e0;
  transform: translateY(-1px);
  box-shadow: 0 6px 20px rgba(91, 54, 245, 0.4);
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (max-width: 768px) {
  .events-grid {
    grid-template-columns: 1fr;
  }
  .form-row {
    grid-template-columns: 1fr;
  }
  .tabs-nav {
    flex-wrap: wrap;
  }
}
</style>
