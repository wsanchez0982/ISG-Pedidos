<script setup>
defineProps({
  orders: Array,
  clients: Array,
  totalSales: Number
})
</script>

<template>
  <section>
    <div class="content-card">
      <h2>Reportes</h2>

      <div class="report-cards">
        <div class="mini-report-card">
          <span>Total pedidos</span>
          <strong>{{ orders.length }}</strong>
        </div>

        <div class="mini-report-card">
          <span>Total clientes</span>
          <strong>{{ clients.length }}</strong>
        </div>

        <div class="mini-report-card">
          <span>Total ventas</span>
          <strong>${{ totalSales.toFixed(2) }}</strong>
        </div>
      </div>
    </div>
  </section>
</template>