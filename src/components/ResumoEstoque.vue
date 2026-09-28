<script setup>
// Este componente é reutilizado 3 vezes no App.vue, cada vez recebendo
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
  flex: 1;
  background-color: #ffffff;
  border-radius: 10px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 12px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
}

.resumo-card--alerta {
  border-left: 4px solid #d9534f;
}

.resumo-card__icone {
  font-size: 1.8rem;
}

.resumo-card__titulo {
  font-size: 0.85rem;
  color: #6b7280;
}

.resumo-card__valor {
  font-size: 1.4rem;
  font-weight: 600;
}
</style>
