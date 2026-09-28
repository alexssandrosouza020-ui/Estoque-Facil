<script setup>
import { ref } from 'vue'

const emit = defineEmits(['adicionar'])

// Estado reativo local do formulário
const nome = ref('')
const categoria = ref('')
const quantidade = ref(1)
const preco = ref(0)

function handleSubmit() {
  if (!nome.value.trim()) return

  emit('adicionar', {
    nome: nome.value,
    categoria: categoria.value || 'Geral',
    quantidade: Number(quantidade.value),
    preco: Number(preco.value)
  })

  // Reseta o formulário após o cadastro
  nome.value = ''
  categoria.value = ''
  quantidade.value = 1
  preco.value = 0
}
</script>

<template>
  <form class="formulario" @submit.prevent="handleSubmit">
    <h2>Cadastrar produto</h2>

    <div class="formulario__linha">
      <input
        v-model="nome"
        type="text"
        placeholder="Nome do produto"
        required
      />
      <input
        v-model="categoria"
        type="text"
        placeholder="Categoria (ex: Bebidas)"
      />
    </div>

    <div class="formulario__linha">
      <input
        v-model="quantidade"
        type="number"
        min="0"
        placeholder="Quantidade"
      />
      <input
        v-model="preco"
        type="number"
        min="0"
        step="0.01"
        placeholder="Preço (R$)"
      />
    </div>

    <button type="submit" class="formulario__botao">
      Adicionar ao estoque
    </button>
  </form>
</template>

<style scoped>
.formulario {
  background-color: #ffffff;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
  margin-bottom: 24px;
}

.formulario h2 {
  font-size: 1.1rem;
  margin-bottom: 12px;
}

.formulario__linha {
  display: flex;
  gap: 12px;
  margin-bottom: 12px;
}

.formulario__linha input {
  flex: 1;
  padding: 10px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  font-size: 0.9rem;
}

.formulario__botao {
  background-color: #1f6f5c;
  color: #ffffff;
  padding: 10px 18px;
  border-radius: 6px;
  font-weight: 600;
}

.formulario__botao:hover {
  background-color: #185a4a;
}
</style>
