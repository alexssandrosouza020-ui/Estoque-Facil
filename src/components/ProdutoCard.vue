<script setup>
// Props: dados que o App.vue envia para cada card
const props = defineProps({
  produto: {
    type: Object,
    required: true
  },
  situacao: {
    type: String,
    default: 'normal' // 'normal', 'baixo' ou 'zerado'
  }
})

// Eventos: o card avisa o App.vue quando o usuário clica em um botão
const emit = defineEmits(['remover', 'movimentar', 'editar'])

function formatarMoeda(valor) {
  return 'R$ ' + Number(valor).toFixed(2).replace('.', ',')
}

function handleRemover() {
  const confirmar = window.confirm('Deseja remover "' + props.produto.nome + '" do estoque?')
  if (confirmar) {
    emit('remover', props.produto.id)
  }
}
</script>

<template>
  <article class="produto-card">
    <div class="produto-card__header">
      <div>
        <span class="produto-card__categoria">{{ produto.categoria }}</span>
        <h3>{{ produto.nome }}</h3>
      </div>

      <!-- v-show: o elemento sempre existe, só alterna entre visível e escondido -->
      <span v-show="situacao !== 'normal'" class="produto-card__badge">
        {{ situacao === 'zerado' ? 'Sem estoque' : 'Estoque baixo' }}
      </span>
    </div>

    <div class="produto-card__dados">
      <span>Qtd: <strong>{{ produto.quantidade }}</strong></span>
      <span>Preço: <strong>{{ formatarMoeda(produto.preco) }}</strong></span>
      <span>Mínimo: <strong>{{ produto.estoqueMinimo }}</strong></span>
    </div>

    <div class="produto-card__acoes">
      <button type="button" class="botao botao--entrada" @click="emit('movimentar', produto.id, 'entrada')">
        Entrada
      </button>
      <button type="button" class="botao botao--saida" @click="emit('movimentar', produto.id, 'saida')">
        Saída
      </button>
      <button type="button" class="botao botao--editar" @click="emit('editar', produto.id)">
        Editar
      </button>
      <button type="button" class="botao botao--remover" @click="handleRemover">
        Remover
      </button>
    </div>
  </article>
</template>

<style scoped>
.produto-card {
  background: linear-gradient(180deg, #ffffff 0%, #f8fafc 100%);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--space-4);
  box-shadow: var(--shadow-soft);
}

.produto-card__header {
  display: flex;
  justify-content: space-between;
  gap: var(--space-3);
  align-items: flex-start;
  margin-bottom: var(--space-3);
}

.produto-card__categoria {
  display: inline-block;
  font-size: 0.7rem;
  color: var(--color-primary);
  background: var(--color-primary-soft);
  border-radius: 999px;
  padding: 5px 8px;
  font-weight: 700;
  margin-bottom: 6px;
}

.produto-card h3 {
  font-size: 1.08rem;
  color: var(--color-text);
}

.produto-card__badge {
  background: var(--color-danger-soft);
  color: var(--color-danger);
  font-size: 0.7rem;
  font-weight: 700;
  padding: 6px 8px;
  border-radius: 999px;
  white-space: nowrap;
}

.produto-card__dados {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: var(--space-3);
  margin-bottom: var(--space-3);
  color: var(--color-text-soft);
  font-size: 0.84rem;
}

.produto-card__dados strong {
  color: var(--color-text);
}

.produto-card__acoes {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-2);
}

.botao {
  border-radius: 10px;
  padding: 9px 12px;
  font-weight: 700;
  transition: transform var(--transition), filter var(--transition);
}

.botao:hover {
  transform: translateY(-1px);
  filter: brightness(0.97);
}

.botao--entrada {
  background: #dbeafe;
  color: #1d4ed8;
}

.botao--saida {
  background: #fef3c7;
  color: #92400e;
}

.botao--editar {
  background: #e0e7ff;
  color: #3730a3;
}

.botao--remover {
  background: var(--color-danger-soft);
  color: var(--color-danger);
}

@media (max-width: 640px) {
  .produto-card__dados {
    grid-template-columns: 1fr;
  }

  .produto-card__acoes .botao {
    flex: 1 1 0;
  }
}
</style>
