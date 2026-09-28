<script setup>
// Componente simples de cabeçalho, sem lógica reativa própria.
defineProps({
  titulo: {
    type: String,
    required: true
  },
  subtitulo: {
    type: String,
    default: ''
  }
})
</script>

<template>
  <header class="app-header">
    <div class="container">
      <h1>{{ titulo }}</h1>
      <p v-if="subtitulo">{{ subtitulo }}</p>
    </div>
  </header>
</template>

<style scoped>
.app-header {
  background-color: #1f6f5c;
  color: #ffffff;
  padding: 24px 0;
}

.app-header h1 {
  font-size: 1.8rem;
}

.app-header p {
  margin-top: 4px;
  opacity: 0.85;
  font-size: 0.95rem;
}
</style>
