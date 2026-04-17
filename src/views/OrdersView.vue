<script setup>
import { computed, ref } from 'vue'

const props = defineProps({
  orders: {
    type: Array,
    default: () => []
  }
})

const search = ref('')
const selectedOrder = ref(null)

const filteredOrders = computed(() => {
  const value = search.value.trim().toLowerCase()

  if (!value) return props.orders

  return props.orders.filter(order =>
    order.clientName.toLowerCase().includes(value) ||
    (order.clientPhone || '').toLowerCase().includes(value) ||
    String(order.id).includes(value)
  )
})

function openOrderDetail(order) {
  selectedOrder.value = order
}

function closeOrderDetail() {
  selectedOrder.value = null
}
</script>

<template>
  <section>
    <div class="orders-shell">
      <div class="content-card orders-header-card">
        <div class="orders-header">
          <div>
            <span class="section-badge">Gestión</span>
            <h2>Pedidos registrados</h2>
            <p>Consulta el historial de pedidos guardados en el sistema.</p>
          </div>

          <div class="orders-search">
            <input
              v-model="search"
              type="text"
              placeholder="Buscar por cliente, teléfono o id"
            >
          </div>
        </div>
      </div>

      <div class="orders-stats-grid">
        <div class="orders-stat-card">
          <span>Total pedidos</span>
          <strong>{{ filteredOrders.length }}</strong>
        </div>

        <div class="orders-stat-card">
          <span>Último registro</span>
          <strong>{{ filteredOrders.length ? filteredOrders[0].date : '-' }}</strong>
        </div>
      </div>

      <div class="content-card orders-table-card">
        <div class="table-wrap premium-table-wrap">
          <table class="data-table premium-orders-table">
            <thead>
              <tr>
                <th>ID</th>
                <th>Fecha</th>
                <th>Cliente</th>
                <th>Teléfono</th>
                <th>Items</th>
                <th>Total</th>
                <th>Estado</th>
              </tr>
            </thead>

            <tbody>
              <tr v-if="filteredOrders.length === 0">
                <td colspan="7" class="empty-cell">No hay pedidos registrados.</td>
              </tr>

              <tr
                v-for="order in filteredOrders"
                :key="order.id"
                class="clickable-row"
                @click="openOrderDetail(order)"
              >
                <td>
                  <span class="order-id">#{{ order.id }}</span>
                </td>

                <td>{{ order.date }}</td>

                <td>
                  <div class="client-cell">
                    <strong>{{ order.clientName }}</strong>
                    <small>{{ order.clientAddress || 'Sin dirección' }}</small>
                  </div>
                </td>

                <td>{{ order.clientPhone || '-' }}</td>

                <td>
                  <span class="items-badge">{{ order.itemsCount }}</span>
                </td>

                <td>
                  <strong class="total-amount">${{ Number(order.total).toFixed(2) }}</strong>
                </td>

                <td>
                  <span class="status-badge">Registrado</span>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- MODAL DETALLE -->
    <div v-if="selectedOrder" class="modal-overlay" @click.self="closeOrderDetail">
      <div class="order-detail-modal">
        <div class="detail-header">
          <div>
            <span class="section-badge">Detalle</span>
            <h2>Pedido #{{ selectedOrder.id }}</h2>
            <p>{{ selectedOrder.date }}</p>
          </div>

          <button class="secondary" @click="closeOrderDetail">Cerrar</button>
        </div>

        <div class="detail-grid">
          <div class="detail-card">
            <h3>Cliente</h3>
            <p><strong>Nombre:</strong> {{ selectedOrder.clientName }}</p>
            <p><strong>Teléfono:</strong> {{ selectedOrder.clientPhone || '-' }}</p>
            <p><strong>Dirección:</strong> {{ selectedOrder.clientAddress || '-' }}</p>
            <p><strong>Observación:</strong> {{ selectedOrder.clientNotes || '-' }}</p>
          </div>

          <div class="detail-card">
            <h3>Resumen</h3>
            <p><strong>Items:</strong> {{ selectedOrder.itemsCount }}</p>
            <p><strong>Subtotal:</strong> ${{ Number(selectedOrder.subtotal).toFixed(2) }}</p>
            <p><strong>IVA 15%:</strong> ${{ Number(selectedOrder.tax).toFixed(2) }}</p>
            <p class="detail-total"><strong>Total:</strong> ${{ Number(selectedOrder.total).toFixed(2) }}</p>
          </div>
        </div>

        <div class="detail-card top-space">
          <h3>Productos del pedido</h3>

          <div class="detail-products">
            <div
              v-for="item in selectedOrder.items"
              :key="item.id"
              class="detail-product-row"
            >
              <div class="detail-product-main">
                <strong>{{ item.name }}</strong>
                <small>{{ item.code }}</small>
              </div>

              <div class="detail-product-side">
                <span>Cant. {{ item.quantity }}</span>
                <span>${{ Number(item.price).toFixed(2) }}</span>
                <strong>${{ (Number(item.price) * Number(item.quantity)).toFixed(2) }}</strong>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>