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

<style scoped>
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(15, 23, 42, 0.55);
  display: grid;
  place-items: center;
  padding: var(--space-4);
  z-index: 10;
}

.modal {
  width: 100%;
  max-width: 420px;
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--space-5);
  box-shadow: var(--shadow-soft);
}

.modal h2 {
  font-size: 1.3rem;
  margin-bottom: var(--space-2);
}

.modal__produto {
  color: var(--color-text-soft);
  font-size: 0.9rem;
  margin-bottom: var(--space-4);
}

.modal__campo {
  display: flex;
  flex-direction: column;
  gap: 6px;
  font-size: 0.8rem;
  font-weight: 600;
  color: var(--color-text-soft);
  margin-bottom: var(--space-3);
}

.modal__campo input,
.modal__campo textarea {
  width: 100%;
  padding: 10px 12px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-surface-muted);
  color: var(--color-text);
  resize: vertical;
}

.modal__acoes {
  display: flex;
  justify-content: flex-end;
  gap: var(--space-2);
  margin-top: var(--space-4);
}

.modal__botao {
  padding: 10px 16px;
  border-radius: var(--radius-sm);
  font-weight: 700;
}

.modal__botao--cancelar {
  background: #e2e8f0;
  color: var(--color-text);
}

.modal__botao--confirmar {
  background: var(--color-primary);
  color: #ffffff;
}
</style>
