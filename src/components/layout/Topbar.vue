<script setup>
defineProps({
  title: String,
  currentUser: String,
  isDarkMode: Boolean
})

const emit = defineEmits(['toggle-theme'])
</script>

<template>
  <header class="topbar">
    <div class="topbar-left">
      <div class="title-group">
        <h1 class="page-title">{{ title }}</h1>
        <p class="page-subtitle">Panel administrativo premium</p>
      </div>
    </div>

    <div class="topbar-right">
      <div class="company-chip">Innova Software Group</div>

      <button class="theme-btn" @click="emit('toggle-theme')">
        {{ isDarkMode ? '☀️' : '🌙' }}
      </button>

      <div class="user-chip">
        <span class="user-label">Usuario</span>
        <strong class="user-name">{{ currentUser }}</strong>
      </div>
    </div>
  </header>
</template>

<style scoped>
.topbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  padding: 18px 24px;
  background:
    linear-gradient(90deg, #111827 0%, #312e81 45%, #2563eb 100%);
  border-bottom: 1px solid rgba(255,255,255,0.08);
  box-shadow: 0 10px 30px rgba(37, 99, 235, 0.12);
}

.topbar-left,
.topbar-right {
  display: flex;
  align-items: center;
}

.topbar-right {
  gap: 12px;
}

.title-group {
  display: flex;
  flex-direction: column;
}

.page-title {
  margin: 0;
  font-size: 24px;
  font-weight: 800;
  color: #ffffff;
  line-height: 1.1;
  letter-spacing: -0.3px;
}

.page-subtitle {
  margin: 4px 0 0;
  font-size: 12px;
  color: #dbeafe;
}

.company-chip,
.theme-btn,
.user-chip {
  border-radius: 16px;
  border: 1px solid rgba(255,255,255,0.12);
  background: rgba(255,255,255,0.08);
  color: #f8fafc;
  backdrop-filter: blur(8px);
}

.company-chip {
  padding: 10px 14px;
  font-size: 12px;
  font-weight: 600;
}

.theme-btn {
  width: 44px;
  height: 44px;
  display: grid;
  place-items: center;
  cursor: pointer;
  font-size: 16px;
  transition: 0.2s ease;
}

.theme-btn:hover {
  background: rgba(255,255,255,0.14);
}

.user-chip {
  padding: 8px 14px;
  display: flex;
  flex-direction: column;
  min-width: 130px;
}

.user-label {
  font-size: 11px;
  color: #dbeafe;
}

.user-name {
  font-size: 13px;
  font-weight: 700;
  color: #ffffff;
  margin-top: 2px;
}

@media (max-width: 1024px) {
  .page-title {
    font-size: 21px;
  }

  .company-chip {
    display: none;
  }
}

@media (max-width: 768px) {
  .topbar {
    flex-direction: column;
    align-items: stretch;
    padding: 16px;
  }

  .topbar-right {
    justify-content: space-between;
    width: 100%;
  }

  .page-title {
    font-size: 20px;
  }

  .page-subtitle {
    font-size: 11px;
  }
}

@media (max-width: 480px) {
  .theme-btn {
    width: 40px;
    height: 40px;
    font-size: 14px;
  }

  .user-name {
    font-size: 12px;
  }
}
</style>