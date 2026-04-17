<script setup>
defineProps({
  clients: Array
})
</script>

<template>
  <section>
    <div class="content-card">
      <h2>Clientes</h2>

      <table class="data-table">
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Teléfono</th>
            <th>Dirección</th>
            <th>Observación</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="clients.length === 0">
            <td colspan="4">No hay clientes registrados.</td>
          </tr>

          <tr v-for="client in clients" :key="client.id">
            <td>{{ client.name }}</td>
            <td>{{ client.phone || '-' }}</td>
            <td>{{ client.address || '-' }}</td>
            <td>{{ client.notes || '-' }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>