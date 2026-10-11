<script setup>
import { ref } from 'vue'

const props = defineProps({
  categorias: {
    type: Array,
    default: () => []
  }
})

const emit = defineEmits(['adicionar'])

const nome = ref('')
const categoria = ref('')
const quantidade = ref(1)
const preco = ref(0)
const estoqueMinimo = ref(5)

function handleSubmit() {
  const nomeValido = nome.value.trim()
  const quantidadeValida = Number(quantidade.value)
  const precoValido = Number(preco.value)

  if (!nomeValido) return
  if (!Number.isFinite(quantidadeValida) || quantidadeValida <= 0) {
    window.alert('Informe uma quantidade válida maior que zero.')
    return
  }
  if (!Number.isFinite(precoValido) || precoValido < 0) {
    window.alert('Informe um preço válido.')
    return
  }

  emit('adicionar', {
    nome: nomeValido,
    categoria: categoria.value.trim() || 'Geral',
    quantidade: quantidadeValida,
    preco: precoValido,
    estoqueMinimo: Number(estoqueMinimo.value) >= 0 ? Number(estoqueMinimo.value) : 5
  })

  nome.value = ''
  categoria.value = ''
  quantidade.value = 1
  preco.value = 0
  estoqueMinimo.value = 5
}
</script>

<template>
  <form class="formulario" @submit.prevent="handleSubmit">
    <div class="formulario__header">
      <div>
        <p class="eyebrow">Cadastro</p>
        <h2>Novo produto</h2>
      </div>
    </div>

    <div class="formulario__linha">
      <label class="campo">
        <span>Nome do produto</span>
        <input v-model="nome" type="text" placeholder="Ex: Teclado mecânico" aria-label="Nome do produto" required />
      </label>

      <label class="campo">
        <span>Categoria</span>
        <input v-model="categoria" type="text" list="categorias-produto" placeholder="Ex: Informática" aria-label="Categoria" />
        <datalist id="categorias-produto">
          <option v-for="categoriaDisponivel in props.categorias" :key="categoriaDisponivel" :value="categoriaDisponivel" />
        </datalist>
      </label>
    </div>

    <div class="formulario__linha">
      <label class="campo">
        <span>Quantidade</span>
        <input v-model.number="quantidade" type="number" min="1" step="1" placeholder="1" aria-label="Quantidade" required />
      </label>

      <label class="campo">
        <span>Preço unitário</span>
        <input v-model.number="preco" type="number" min="0" step="0.01" placeholder="0,00" aria-label="Preço unitário" required />
      </label>
    </div>

    <div class="formulario__linha">
      <label class="campo">
        <span>Estoque mínimo (gera alerta)</span>
        <input v-model.number="estoqueMinimo" type="number" min="0" step="1" placeholder="5" aria-label="Estoque mínimo" required />
      </label>
    </div>

    <button type="submit" class="formulario__botao">
      Adicionar ao estoque
    </button>
  </form>
</template>

<style scoped>
.formulario {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--space-5);
  box-shadow: var(--shadow-soft);
}

.formulario__header {
  margin-bottom: var(--space-4);
}

.eyebrow {
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-size: 0.72rem;
  color: var(--color-primary);
  font-weight: 700;
  margin-bottom: 4px;
}

.formulario h2 {
  font-size: 1.4rem;
  margin: 0;
  color: var(--color-text);
}

.formulario__linha {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: var(--space-3);
  margin-bottom: var(--space-3);
}

.campo {
  display: flex;
  flex-direction: column;
  gap: 6px;
  font-size: 0.75rem;
  color: var(--color-text-soft);
  font-weight: 600;
}

.campo input {
  width: 100%;
  min-height: 44px;
  padding: 10px 12px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-surface-muted);
  color: var(--color-text);
  transition: border-color var(--transition), box-shadow var(--transition);
}

.campo input:focus {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(15, 118, 110, 0.1);
}

.formulario__botao {
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-primary-strong) 100%);
  color: #fff;
  padding: 12px 18px;
  border-radius: var(--radius-sm);
  font-weight: 700;
  transition: transform var(--transition), filter var(--transition);
  min-height: 46px;
}

.formulario__botao:hover {
  transform: translateY(-1px);
  filter: brightness(1.02);
}

@media (max-width: 640px) {
  .formulario__linha {
    grid-template-columns: 1fr;
  }
}
</style>
