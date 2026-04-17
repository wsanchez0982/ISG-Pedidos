<script setup>
import { ref, computed, watch } from 'vue'
import Sidebar from './components/layout/Sidebar.vue'
import Topbar from './components/layout/Topbar.vue'

import DashboardView from './views/DashboardView.vue'
import CreateOrderView from './views/CreateOrderView.vue'
import OrdersView from './views/OrdersView.vue'
import ClientsView from './views/ClientsView.vue'
import ProductsView from './views/ProductsView.vue'
import ReportsView from './views/ReportsView.vue'

import { productsData } from './data/products'
import { getStorageData, setStorageData } from './utils/storage'

const currentSection = ref('inicio')
const currentUser = ref(localStorage.getItem('username') || 'Admin')
const isDarkMode = ref(localStorage.getItem('theme') === 'dark')

const products = ref(productsData)
const orders = ref(getStorageData('orders', []))
const clients = ref(getStorageData('clients', []))

const pageTitle = computed(() => {
  const titles = {
    inicio: 'Inicio',
    crearPedido: 'Nuevo Pedido',
    pedidos: 'Pedidos',
    clientes: 'Clientes',
    productos: 'Productos',
    reportes: 'Reportes'
  }

  return titles[currentSection.value] || 'Sistema'
})

const totalSales = computed(() => {
  return orders.value.reduce((acc, order) => acc + Number(order.total || 0), 0)
})

function goToSection(section) {
  currentSection.value = section
}

function toggleTheme() {
  isDarkMode.value = !isDarkMode.value
}

function saveOrder(orderData) {
  const newOrder = {
    ...orderData,
    id: Date.now(),
    date: new Date().toLocaleString('es-EC')
  }

  orders.value.unshift(newOrder)

  const exists = clients.value.some(client =>
    client.name.toLowerCase() === orderData.clientName.toLowerCase() &&
    (client.phone || '') === (orderData.clientPhone || '')
  )

  if (!exists) {
    clients.value.unshift({
      id: Date.now() + 1,
      name: orderData.clientName,
      phone: orderData.clientPhone,
      address: orderData.clientAddress,
      notes: orderData.clientNotes
    })
  }

  currentSection.value = 'pedidos'
}

watch(orders, value => setStorageData('orders', value), { deep: true })
watch(clients, value => setStorageData('clients', value), { deep: true })

watch(isDarkMode, value => {
  localStorage.setItem('theme', value ? 'dark' : 'light')
  document.body.classList.toggle('dark-mode', value)
})

document.body.classList.toggle('dark-mode', isDarkMode.value)
</script>

<template>
  <div class="layout-shell">
    <Sidebar
      :current-section="currentSection"
      @navigate="goToSection"
    />

    <div class="main-area">
      <Topbar
        :title="pageTitle"
        :current-user="currentUser"
        :is-dark-mode="isDarkMode"
        @toggle-theme="toggleTheme"
      />

      <main class="content-area">
        <DashboardView
          v-if="currentSection === 'inicio'"
          :orders="orders"
          :clients="clients"
          :total-sales="totalSales"
          @navigate="goToSection"
        />

        <CreateOrderView
          v-else-if="currentSection === 'crearPedido'"
          :products="products"
          @save-order="saveOrder"
          @back="goToSection('inicio')"
        />

        <OrdersView
          v-else-if="currentSection === 'pedidos'"
          :orders="orders"
        />

        <ClientsView
          v-else-if="currentSection === 'clientes'"
          :clients="clients"
        />

        <ProductsView
          v-else-if="currentSection === 'productos'"
          :products="products"
        />

        <ReportsView
          v-else-if="currentSection === 'reportes'"
          :orders="orders"
          :clients="clients"
          :total-sales="totalSales"
        />
      </main>
    </div>
  </div>
</template>