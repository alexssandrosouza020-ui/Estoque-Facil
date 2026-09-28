<script setup>
// Este componente é reutilizado uma vez para cada produto da lista,
// através de v-for no App.vue, recebendo dados diferentes por props.
const props = defineProps({
  produto: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['remover'])

function handleRemover() {
  emit('remover', props.produto.id)
}
</script>

<template>
  <div class="produto-card">
    <div class="produto-card__info">
      <h3>{{ produto.nome }}</h3>
      <p class="produto-card__categoria">{{ produto.categoria }}</p>
    </div>

    <div class="produto-card__dados">
      <span>Qtd: <strong>{{ produto.quantidade }}</strong></span>
      <span>R$ {{ produto.preco.toFixed(2) }}</span>
    </div>

    <span v-show="produto.quantidade < 5" class="produto-card__badge">
      Estoque baixo
    </span>

    <button class="produto-card__remover" @click="handleRemover">
      Remover
    </button>
  </div>
</template>

<style scoped>
.produto-card {
  background-color: #ffffff;
  border-radius: 10px;
  padding: 14px 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
  margin-bottom: 10px;
  position: relative;
}

.produto-card__info {
  flex: 1;
}

.produto-card__categoria {
  font-size: 0.8rem;
  color: #6b7280;
}

.produto-card__dados {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  font-size: 0.9rem;
  gap: 2px;
  min-width: 90px;
}

.produto-card__badge {
  background-color: #fdecea;
  color: #d9534f;
  font-size: 0.75rem;
  padding: 4px 8px;
  border-radius: 6px;
  font-weight: 600;
}

.produto-card__remover {
  background-color: #f4f6f8;
  color: #d9534f;
  padding: 6px 10px;
  border-radius: 6px;
  font-size: 0.85rem;
}

.produto-card__remover:hover {
  background-color: #fdecea;
}
</style>
