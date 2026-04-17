<script setup>
defineProps({
  message: {
    type: String,
    default: 'No hay datos'
  }
})
</script>

<template>
  <p class="empty-state">{{ message }}</p>
</template>

<style scoped>
.empty-state {
  text-align: center;
  color: #6b7280;
  padding: 20px 0;
}
</style>