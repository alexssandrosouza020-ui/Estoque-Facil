<script setup>
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
    <div class="container app-header__content">
      <div>
        <p class="app-header__tag">Dashboard de estoque</p>
        <h1>{{ titulo }}</h1>
      </div>
      <p v-if="subtitulo" class="app-header__subtitle">{{ subtitulo }}</p>
    </div>
  </header>
</template>

<style scoped>
.app-header {
  background: linear-gradient(135deg, #0f766e 0%, #134e4a 100%);
  color: #ffffff;
  padding: 26px 0;
  box-shadow: 0 10px 24px rgba(15, 118, 110, 0.2);
}

.app-header__content {
  display: flex;
  justify-content: space-between;
  align-items: end;
  gap: 16px;
}

.app-header__tag {
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-size: 0.68rem;
  opacity: 0.8;
  margin-bottom: 6px;
}

.app-header h1 {
  font-size: clamp(1.9rem, 2vw, 2.5rem);
  line-height: 1.2;
}

.app-header__subtitle {
  max-width: 420px;
  opacity: 0.9;
  font-size: 0.95rem;
  text-align: right;
}

@media (max-width: 640px) {
  .app-header__content {
    flex-direction: column;
    align-items: flex-start;
  }

  .app-header__subtitle {
    text-align: left;
  }
}
</style>
