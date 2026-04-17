<script setup>
defineProps({
  products: Array
})
</script>

<template>
  <section>
    <div class="content-card">
      <h2>Productos</h2>

      <div class="product-list">
        <div class="product-card" v-for="product in products" :key="product.id">
          <span>{{ product.name }} - {{ product.code }}</span>
          <strong>${{ product.price.toFixed(2) }}</strong>
        </div>
      </div>
    </div>
  </section>
</template>