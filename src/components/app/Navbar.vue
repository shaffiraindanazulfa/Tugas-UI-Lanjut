<template>
  <nav class="navbar" :class="{ scrolled: isScrolled }">
    <div class="navbar-container">

      <!-- Logo -->
      <router-link to="/" class="logo">
        <span class="logo-icon">🎪</span>
        <span class="logo-text">Gatherly</span>
      </router-link>

      <!-- Center Navigation -->
      <ul class="nav-menu">
        <li class="nav-item">
          <router-link to="/" class="nav-link" exact-active-class="active">Home</router-link>
        </li>

        <li class="nav-item">
          <router-link to="/about" class="nav-link" active-class="active">About</router-link>
        </li>

        <!-- Browse with dropdown -->
        <li class="nav-item has-dropdown">
          <router-link to="/browse" class="nav-link" active-class="active">
            Browse
            <span class="dropdown-indicator">▾</span>
          </router-link>

          <ul class="dropdown-menu">
            <!-- Event List with submenu -->
            <li class="dropdown-item has-submenu">
              <router-link to="/browse/events" class="dropdown-link">
                Event List
                <span class="arrow">›</span>
              </router-link>
              <ul class="submenu">
                <li>
                  <router-link to="/browse/events/1" class="dropdown-link">Event Detail (Example)</router-link>
                </li>
                <li>
                  <router-link to="/browse/events/2" class="dropdown-link">Event Detail 2</router-link>
                </li>
              </ul>
            </li>

            <li class="dropdown-item">
              <router-link to="/browse/category" class="dropdown-link">Category</router-link>
            </li>
          </ul>
        </li>

        <li class="nav-item">
          <router-link to="/contact" class="nav-link" active-class="active">Contact</router-link>
        </li>

        <li class="nav-item">
          <router-link to="/dashboard" class="nav-link" active-class="active">Organizer Dashboard</router-link>
        </li>
      </ul>

      <!-- Right section -->
      <div class="nav-right">
        <div class="lang-selector">
          <span>🌐</span>
          <span>EN</span>
          <span class="chevron">▾</span>
        </div>

        <button class="hamburger-btn" aria-label="Menu">
          <span>☰</span>
        </button>
      </div>

    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const isScrolled = ref(false)

function onScroll() {
  isScrolled.value = window.scrollY > 20
}

onMounted(() => window.addEventListener('scroll', onScroll))
onUnmounted(() => window.removeEventListener('scroll', onScroll))
</script>

<style scoped>
/* ── Navbar shell ── */
.navbar {
  width: 100%;
  background: var(--nav-bg, #1c1948);
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.15);
  position: sticky;
  top: 0;
  z-index: 999;
  transition: background 0.3s ease, box-shadow 0.3s ease;
}

.navbar.scrolled {
  background: rgba(28, 25, 72, 0.95);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.25);
}

.navbar-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  max-width: 1440px;
  margin: 0 auto;
  padding: 0 2rem;
  height: 64px;
}

/* ── Logo ── */
.logo {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  text-decoration: none;
  color: #fff;
  flex-shrink: 0;
  transition: opacity 0.2s;
}
.logo:hover { opacity: 0.85; text-decoration: none; }

.logo-icon { font-size: 1.4rem; }

.logo-text {
  font-size: 1.2rem;
  font-weight: 700;
  letter-spacing: -0.01em;
  color: #fff;
}

/* ── Nav menu ── */
.nav-menu {
  display: flex;
  align-items: center;
  gap: 0.25rem;
  list-style: none;
  margin: 0;
  padding: 0;
  flex: 1;
  justify-content: center;
}

.nav-item {
  position: relative;
}

.nav-link {
  display: flex;
  align-items: center;
  gap: 0.35rem;
  text-decoration: none;
  color: rgba(255, 255, 255, 0.85);
  font-size: 0.95rem;
  font-weight: 500;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.nav-link:hover {
  color: #fff;
  background: rgba(255, 255, 255, 0.1);
  text-decoration: none;
}

.nav-link.active {
  background: var(--primary, #6644ff);
  color: #fff;
  font-weight: 600;
  box-shadow: 0 2px 12px rgba(102, 68, 255, 0.35);
}

.dropdown-indicator {
  font-size: 0.75rem;
  transition: transform 0.25s ease;
  line-height: 1;
}

.nav-item.has-dropdown:hover .dropdown-indicator {
  transform: rotate(180deg);
}

/* ── Dropdown level 1 ── */
.dropdown-menu {
  display: none;
  position: absolute;
  top: calc(100% + 8px);
  left: 0;
  min-width: 200px;
  background: #fff;
  border: 1px solid #e5e5e5;
  border-radius: 10px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
  list-style: none;
  padding: 6px;
  margin: 0;
  animation: dropIn 0.18s ease;
}

.nav-item.has-dropdown:hover .dropdown-menu {
  display: block;
}

@keyframes dropIn {
  from { opacity: 0; transform: translateY(-6px); }
  to   { opacity: 1; transform: translateY(0); }
}

.dropdown-item {
  position: relative;
}

.dropdown-link {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.6rem 1rem;
  text-decoration: none;
  color: #333;
  font-size: 0.92rem;
  font-weight: 500;
  border-radius: 7px;
  transition: background 0.15s ease, color 0.15s ease;
}

.dropdown-link:hover {
  background: var(--primary-light, rgba(102, 68, 255, 0.08));
  color: var(--primary, #6644ff);
  text-decoration: none;
}

.dropdown-link.router-link-active {
  color: var(--primary, #6644ff);
  font-weight: 600;
}

.arrow {
  font-size: 1rem;
  color: #aaa;
}

/* ── Submenu level 2 ── */
.submenu {
  display: none;
  position: absolute;
  top: 0;
  left: 100%;
  min-width: 210px;
  background: #fff;
  border: 1px solid #e5e5e5;
  border-radius: 10px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
  list-style: none;
  padding: 6px;
  margin: 0 0 0 4px;
  animation: dropIn 0.18s ease;
}

.dropdown-item.has-submenu:hover .submenu {
  display: block;
}

/* ── Right section ── */
.nav-right {
  display: flex;
  align-items: center;
  gap: 1rem;
  flex-shrink: 0;
}

.lang-selector {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  color: rgba(255, 255, 255, 0.85);
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  padding: 0.4rem 0.75rem;
  border-radius: 8px;
  transition: background 0.2s;
  user-select: none;
}

.lang-selector:hover {
  background: rgba(255, 255, 255, 0.1);
}

.chevron {
  font-size: 0.75rem;
  transition: transform 0.25s;
}

.lang-selector:hover .chevron {
  transform: rotate(180deg);
}

.hamburger-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 8px;
  color: #fff;
  font-size: 1.1rem;
  cursor: pointer;
  transition: background 0.2s;
}

.hamburger-btn:hover {
  background: rgba(255, 255, 255, 0.18);
}

/* ── Responsive ── */
@media (max-width: 900px) {
  .nav-menu { gap: 0; }
  .nav-link { padding: 0.5rem 0.75rem; font-size: 0.88rem; }
}

@media (max-width: 768px) {
  .nav-menu { display: none; }
  .lang-selector { display: none; }
}
</style>