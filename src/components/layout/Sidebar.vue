<script setup>
import logo from '../../assets/logo.png'

defineProps({
  currentSection: String
})

const emit = defineEmits(['navigate'])

const menuItems = [
  { key: 'inicio', label: 'Inicio', icon: 'fa-solid fa-house' },
  { key: 'crearPedido', label: 'Nuevo Pedido', icon: 'fa-solid fa-cart-plus' },
  { key: 'pedidos', label: 'Pedidos', icon: 'fa-solid fa-file-invoice' },
  { key: 'clientes', label: 'Clientes', icon: 'fa-solid fa-users' },
  { key: 'productos', label: 'Productos', icon: 'fa-solid fa-boxes-stacked' },
  { key: 'reportes', label: 'Reportes', icon: 'fa-solid fa-chart-column' }
]
</script>

<template>
  <aside class="sidebar">
    <div class="sidebar-brand">
      <img :src="logo" alt="Innova Software Group" class="logo-img">
    </div>

    <nav class="sidebar-nav">
      <button
        v-for="item in menuItems"
        :key="item.key"
        class="nav-item"
        :class="{ active: currentSection === item.key }"
        @click="emit('navigate', item.key)"
      >
        <i :class="item.icon"></i>
        <span>{{ item.label }}</span>
      </button>
    </nav>

    <div class="sidebar-footer">
      <small>Innova Software Group</small>
      <strong>Versión 1.0.0</strong>
    </div>
  </aside>
</template>

<style scoped>
.sidebar {
  width: 280px;
  min-height: 100vh;
  background:
    linear-gradient(180deg, #0b1120 0%, #111827 45%, #1e1b4b 100%);
  color: #e5e7eb;
  display: flex;
  flex-direction: column;
  padding: 18px;
  border-right: 1px solid rgba(255,255,255,0.08);
  box-shadow: 10px 0 30px rgba(0, 0, 0, 0.18);
}

.sidebar-brand {
  text-align: center;
  padding-bottom: 20px;
  border-bottom: 1px solid rgba(255,255,255,0.08);
  margin-bottom: 20px;
}

.logo-img {
  width: 100%;
  max-width: 180px;
  object-fit: contain;
}

.sidebar-nav {
  display: flex;
  flex-direction: column;
  gap: 10px;
  flex: 1;
}

.nav-item {
  position: relative;
  border: 1px solid transparent;
  background: transparent;
  color: #cbd5e1;
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 14px 16px;
  border-radius: 16px;
  cursor: pointer;
  transition: all 0.25s ease;
  text-align: left;
  font-size: 14px;
  font-weight: 600;
}

.nav-item i {
  width: 20px;
  text-align: center;
  font-size: 15px;
}

.nav-item:hover {
  background: rgba(255,255,255,0.05);
  border-color: rgba(255,255,255,0.06);
  color: #ffffff;
  transform: translateX(2px);
}

.nav-item.active {
  color: #ffffff;
  background: linear-gradient(
    90deg,
    rgba(124, 58, 237, 0.24),
    rgba(37, 99, 235, 0.14)
  );
  border-color: rgba(147, 51, 234, 0.24);
  box-shadow:
    inset 3px 0 0 #a855f7,
    0 8px 22px rgba(124, 58, 237, 0.18);
}

.nav-item.active::before {
  content: "";
  position: absolute;
  left: 0;
  top: 10px;
  bottom: 10px;
  width: 4px;
  border-radius: 0 8px 8px 0;
  background: linear-gradient(180deg, #7c3aed, #9333ea, #2563eb);
}

.sidebar-footer {
  padding-top: 20px;
  border-top: 1px solid rgba(255,255,255,0.08);
  text-align: center;
  line-height: 1.4;
}

.sidebar-footer small {
  display: block;
  color: #94a3b8;
  margin-bottom: 4px;
  font-size: 11px;
}

.sidebar-footer strong {
  color: #c4b5fd;
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 0.3px;
}

@media (max-width: 1024px) {
  .sidebar {
    width: 92px;
    padding: 12px;
    align-items: center;
  }

  .logo-img {
    max-width: 52px;
  }

  .nav-item {
    justify-content: center;
    padding: 14px;
  }

  .nav-item span,
  .sidebar-footer {
    display: none;
  }
}

@media (max-width: 768px) {
  .sidebar {
    width: 100%;
    min-height: auto;
    padding: 12px;
    border-right: none;
    border-bottom: 1px solid rgba(255,255,255,0.08);
  }

  .sidebar-brand {
    margin-bottom: 14px;
    padding-bottom: 14px;
  }

  .logo-img {
    max-width: 140px;
  }

  .sidebar-nav {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
  }

  .nav-item {
    justify-content: center;
    padding: 12px;
  }

  .nav-item span {
    display: none;
  }
}

@media (max-width: 480px) {
  .sidebar-nav {
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>