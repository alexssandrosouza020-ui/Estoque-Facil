<script setup>
import { ref } from 'vue'

// Props: o produto que será editado (enviado pelo App.vue)
const props = defineProps({
  produto: {
    type: Object,
    required: true
  }
})

// Eventos: o modal avisa o App.vue o que o usuário decidiu
const emit = defineEmits(['salvar', 'cancelar'])

// Variáveis reativas locais, já preenchidas com os dados atuais do produto.
// Produtos antigos sem estoque mínimo usam 0 como valor inicial.
const nome = ref(props.produto.nome)
const preco = ref(props.produto.preco)
const estoqueMinimo = ref(props.produto.estoqueMinimo ?? 0)

// Mensagem de erro de validação (vazia = sem erro)
const erro = ref('')

function numeroValido(valor) {
  // v-model.number devolve '' quando o campo está vazio
  return valor !== '' && valor !== null && !Number.isNaN(Number(valor)) && Number(valor) >= 0
}

function handleSalvar() {
  const nomeLimpo = String(nome.value).trim()

  if (nomeLimpo === '') {
    erro.value = 'O nome do produto é obrigatório.'
    return
  }

  if (!numeroValido(preco.value)) {
    erro.value = 'Informe um preço maior ou igual a zero.'
    return
  }

  if (!numeroValido(estoqueMinimo.value)) {
    erro.value = 'Informe um estoque mínimo maior ou igual a zero.'
    return
  }

  erro.value = ''

  emit('salvar', {
    nome: nomeLimpo,
    preco: Number(preco.value),
    estoqueMinimo: Number(estoqueMinimo.value)
  })
}
</script>

<template>
  <div class="modal-overlay" @click.self="emit('cancelar')">
    <div class="modal">
      <h2>Editar produto</h2>
      <p class="modal__produto">
        {{ produto.categoria }} — em estoque: <strong>{{ produto.quantidade }}</strong>
      </p>

      <label class="modal__campo">
        <span>Nome do produto</span>
        <input v-model="nome" type="text" placeholder="Nome do produto" />
      </label>

      <label class="modal__campo">
        <span>Preço unitário (R$)</span>
        <input v-model.number="preco" type="number" min="0" step="0.01" />
      </label>

      <label class="modal__campo">
        <span>Estoque mínimo</span>
        <input v-model.number="estoqueMinimo" type="number" min="0" step="1" />
      </label>

      <p v-if="erro" class="modal__erro" role="alert">{{ erro }}</p>

      <div class="modal__acoes">
        <button type="button" class="modal__botao modal__botao--cancelar" @click="emit('cancelar')">
          Cancelar
        </button>
        <button type="button" class="modal__botao modal__botao--confirmar" @click="handleSalvar">
          Salvar alterações
        </button>
      </div>
    </div>
  </div>
</template>
