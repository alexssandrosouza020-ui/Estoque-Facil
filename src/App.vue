<script setup>
import { ref, computed } from 'vue'
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'
import ResumoEstoque from './components/ResumoEstoque.vue'
import FormularioProduto from './components/FormularioProduto.vue'
import ProdutoCard from './components/ProdutoCard.vue'

// Lista reativa de produtos em estoque (estado principal da aplicação)
const produtos = ref([
  { id: 1, nome: 'Caneta Azul', categoria: 'Papelaria', quantidade: 20, preco: 2.5 },
  { id: 2, nome: 'Caderno Universitário', categoria: 'Papelaria', quantidade: 3, preco: 15.9 },
  { id: 3, nome: 'Mouse sem fio', categoria: 'Informática', quantidade: 8, preco: 45.0 },
  { id: 4, nome: 'Cabo HDMI', categoria: 'Informática', quantidade: 2, preco: 22.3 }
])

let proximoId = 5

// Propriedades computadas usadas nos cards de resumo (reúso do ResumoEstoque)
const totalProdutos = computed(() => produtos.value.length)

const valorTotalEstoque = computed(() => {
  const total = produtos.value.reduce(
    (soma, produto) => soma + produto.quantidade * produto.preco,
    0
  )
  return `R$ ${total.toFixed(2)}`
})

const produtosBaixoEstoque = computed(
  () => produtos.value.filter((produto) => produto.quantidade < 5).length
)

function adicionarProduto(novoProduto) {
  produtos.value.push({ id: proximoId++, ...novoProduto })
}

function removerProduto(id) {
  produtos.value = produtos.value.filter((produto) => produto.id !== id)
}
</script>

<template>
  <AppHeader
    titulo="Estoque Fácil"
    subtitulo="Controle simples de produtos e quantidades"
  />

  <main class="container conteudo">
    <!-- Reúso do componente ResumoEstoque, com props diferentes em cada uso -->
    <section class="resumos">
      <ResumoEstoque titulo="Produtos cadastrados" :valor="totalProdutos" icone="📦" />
      <ResumoEstoque titulo="Valor total em estoque" :valor="valorTotalEstoque" icone="💰" />
      <ResumoEstoque
        titulo="Itens com estoque baixo"
        :valor="produtosBaixoEstoque"
        icone="⚠️"
        :destaque="produtosBaixoEstoque > 0"
      />
    </section>

    <FormularioProduto @adicionar="adicionarProduto" />

    <section class="lista-produtos">
      <h2>Produtos em estoque</h2>

      <p v-if="produtos.length === 0" class="lista-produtos__vazio">
        Nenhum produto cadastrado ainda.
      </p>

      <!-- Reúso do componente ProdutoCard, um para cada item da lista -->
      <ProdutoCard
        v-for="produto in produtos"
        :key="produto.id"
        :produto="produto"
        @remover="removerProduto"
      />
    </section>
  </main>

  <AppFooter />
</template>

<style scoped>
.conteudo {
  flex: 1;
  padding: 24px 20px 40px;
}

.resumos {
  display: flex;
  gap: 16px;
  margin-bottom: 24px;
  flex-wrap: wrap;
}

.lista-produtos h2 {
  font-size: 1.1rem;
  margin-bottom: 12px;
}

.lista-produtos__vazio {
  color: #6b7280;
  font-style: italic;
}
</style>
