<script setup>
// Este componente é reutilizado quatro vezes no App.vue, cada vez recebendo
// props diferentes (título, valor e ícone), demonstrando reúso de componente.
defineProps({
  titulo: {
    type: String,
    required: true
  },
  valor: {
    type: [String, Number],
    required: true
  },
  icone: {
    type: String,
    default: '📦'
  },
  destaque: {
    type: Boolean,
    default: false
  }
})
</script>

<template>
  <div class="resumo-card" :class="{ 'resumo-card--alerta': destaque }">
    <span class="resumo-card__icone">{{ icone }}</span>
    <div>
      <p class="resumo-card__titulo">{{ titulo }}</p>
      <p class="resumo-card__valor">{{ valor }}</p>
    </div>
  </div>
</template>

<style scoped>
.resumo-card {
  height: 100%;
  min-height: 108px;
  background-color: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 12px;
  box-shadow: var(--shadow-soft);
}

.resumo-card--alerta {
  border-color: #f1c7c3;
  background: #fffafa;
}

.resumo-card__icone {
  display: grid;
  place-items: center;
  flex: 0 0 40px;
  width: 40px;
  height: 40px;
  border-radius: var(--radius-sm);
  background: var(--color-primary-soft);
  font-size: 1.25rem;
}

.resumo-card__titulo {
  font-size: 0.85rem;
  color: var(--color-text-muted);
}

.resumo-card__valor {
  color: var(--color-text);
  font-size: 1.5rem;
  font-weight: 700;
  line-height: 1.25;
  overflow-wrap: anywhere;
}
</style>
