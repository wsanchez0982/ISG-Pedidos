<script setup>
defineProps({
  orders: {
    type: Array,
    default: () => []
  },
  clients: {
    type: Array,
    default: () => []
  },
  totalSales: {
    type: Number,
    default: 0
  }
})

const emit = defineEmits(['navigate'])
</script>

<template>
  <section>
    <div class="dashboard-shell">
      <!-- KPIs -->
      <div class="kpi-grid">
        <div class="kpi-card">
          <div class="kpi-icon">📋</div>
          <div class="kpi-info">
            <span>Total pedidos</span>
            <strong>{{ orders.length }}</strong>
          </div>
        </div>

        <div class="kpi-card">
          <div class="kpi-icon">👥</div>
          <div class="kpi-info">
            <span>Total clientes</span>
            <strong>{{ clients.length }}</strong>
          </div>
        </div>

        <div class="kpi-card">
          <div class="kpi-icon">💰</div>
          <div class="kpi-info">
            <span>Ventas acumuladas</span>
            <strong>${{ totalSales.toFixed(2) }}</strong>
          </div>
        </div>
      </div>
      
      <!-- Accesos -->
      <div class="content-card quick-card">
      <div class="section-hero">
        <span class="section-badge">Accesos Rápidos</span>
      </div>

        <div class="shortcut-grid">
          <button class="shortcut-card" @click="emit('navigate','crearPedido')">
            <div class="shortcut-icon">🛒</div>
            <strong>Crear pedido</strong>
            <p>Registrar una nueva orden.</p>
          </button>

          <button class="shortcut-card" @click="emit('navigate','pedidos')">
            <div class="shortcut-icon">📋</div>
            <strong>Historial</strong>
            <p>Consultar pedidos guardados.</p>
          </button>

          <button class="shortcut-card" @click="emit('navigate','clientes')">
            <div class="shortcut-icon">👥</div>
            <strong>Clientes</strong>
            <p>Administrar clientes.</p>
          </button>

          <button class="shortcut-card" @click="emit('navigate','productos')">
            <div class="shortcut-icon">📦</div>
            <strong>Productos</strong>
            <p>Ver catálogo disponible.</p>
          </button>

          <button class="shortcut-card" @click="emit('navigate','reportes')">
            <div class="shortcut-icon">📈</div>
            <strong>Reportes</strong>
            <p>Indicadores y métricas.</p>
          </button>
        </div>
      </div>
    </div>
  </section>
</template>