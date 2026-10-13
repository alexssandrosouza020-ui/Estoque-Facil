<script setup>
import { ref } from 'vue'

// Props: informações que o App.vue envia para o modal
const props = defineProps({
  produto: {
    type: Object,
    required: true
  },
  tipo: {
    type: String,
    required: true // 'entrada' ou 'saida'
  }
})

// Eventos: o modal avisa o App.vue o que o usuário decidiu
const emit = defineEmits(['confirmar', 'cancelar'])

// Variáveis reativas locais, ligadas aos campos com v-model
const quantidade = ref(1)
const observacao = ref('')

function handleConfirmar() {
  if (!quantidade.value || quantidade.value <= 0) {
    window.alert('Informe uma quantidade maior que zero.')
    return
  }

  if (props.tipo === 'saida' && quantidade.value > props.produto.quantidade) {
    window.alert('Quantidade maior do que o estoque disponível.')
    return
  }

  emit('confirmar', {
    quantidade: quantidade.value,
    observacao: observacao.value.trim()
  })
}
</script>

<template>
  <div class="modal-overlay" @click.self="emit('cancelar')">
    <div class="modal">
      <h2>{{ tipo === 'entrada' ? 'Registrar entrada' : 'Registrar saída' }}</h2>
      <p class="modal__produto">
        {{ produto.nome }} — em estoque: <strong>{{ produto.quantidade }}</strong>
      </p>

      <label class="modal__campo">
        <span>Quantidade</span>
        <input v-model.number="quantidade" type="number" min="1" step="1" />
      </label>

      <label class="modal__campo">
        <span>Observação (opcional)</span>
        <textarea v-model="observacao" rows="3" placeholder="Ex: compra do fornecedor"></textarea>
      </label>

      <div class="modal__acoes">
        <button type="button" class="modal__botao modal__botao--cancelar" @click="emit('cancelar')">
          Cancelar
        </button>
        <button type="button" class="modal__botao modal__botao--confirmar" @click="handleConfirmar">
          Confirmar
        </button>
      </div>
    </div>
  </div>
</template>
