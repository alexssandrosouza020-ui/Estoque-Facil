<script setup>
import { ref } from 'vue'
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'
import ResumoEstoque from './components/ResumoEstoque.vue'
import FormularioProduto from './components/FormularioProduto.vue'
import ProdutoCard from './components/ProdutoCard.vue'
import ModalMovimento from './components/ModalMovimento.vue'

// ---------------------------------------------------------------
// ESTADO REATIVO (ref): quando um valor muda, a tela atualiza sozinha
// ---------------------------------------------------------------
const produtos = ref([
  { id: 1, nome: 'Caneta Azul', categoria: 'Papelaria', quantidade: 20, preco: 2.5, estoqueMinimo: 5 },
  { id: 2, nome: 'Caderno Universitário', categoria: 'Papelaria', quantidade: 3, preco: 15.9, estoqueMinimo: 5 },
  { id: 3, nome: 'Mouse sem fio', categoria: 'Informática', quantidade: 8, preco: 45.0, estoqueMinimo: 5 },
  { id: 4, nome: 'Cabo HDMI', categoria: 'Informática', quantidade: 0, preco: 22.3, estoqueMinimo: 5 }
])

const historico = ref([])          // lista de movimentações registradas
const busca = ref('')              // texto digitado na pesquisa
const categoriaFiltro = ref('')    // categoria escolhida no filtro
const movimentoEmAberto = ref(null) // null = modal fechado; { id, tipo } = modal aberto

let proximoIdProduto = 5
let proximoIdMovimento = 1

// ---------------------------------------------------------------
// FUNÇÕES AUXILIARES (usadas no template)
// ---------------------------------------------------------------
function formatarMoeda(valor) {
  return 'R$ ' + Number(valor).toFixed(2).replace('.', ',')
}

function situacaoEstoque(produto) {
  if (produto.quantidade === 0) return 'zerado'
  if (produto.quantidade < produto.estoqueMinimo) return 'baixo'
  return 'normal'
}

function totalUnidades() {
  let soma = 0
  for (const produto of produtos.value) {
    soma += produto.quantidade
  }
  return soma
}

function valorTotalEstoque() {
  let soma = 0
  for (const produto of produtos.value) {
    soma += produto.quantidade * produto.preco
  }
  return formatarMoeda(soma)
}

function produtosEmAlerta() {
  return produtos.value.filter((produto) => situacaoEstoque(produto) !== 'normal')
}

function listaCategorias() {
  const nomes = []
  for (const produto of produtos.value) {
    if (!nomes.includes(produto.categoria)) {
      nomes.push(produto.categoria)
    }
  }
  return nomes.sort()
}

function produtosFiltrados() {
  const termo = busca.value.trim().toLowerCase()
  return produtos.value.filter((produto) => {
    const nomeConfere = produto.nome.toLowerCase().includes(termo)
    const categoriaConfere = categoriaFiltro.value === '' || produto.categoria === categoriaFiltro.value
    return nomeConfere && categoriaConfere
  })
}

function limparFiltros() {
  busca.value = ''
  categoriaFiltro.value = ''
}

function rotuloMovimento(tipo) {
  if (tipo === 'entrada') return 'Entrada'
  if (tipo === 'saida') return 'Saída'
  if (tipo === 'cadastro') return 'Cadastro'
  return 'Remoção'
}

// ---------------------------------------------------------------
// AÇÕES (chamadas pelos eventos dos componentes filhos)
// ---------------------------------------------------------------
function registrarHistorico(tipo, nomeProduto, quantidade, observacao) {
  // unshift coloca o item mais novo no começo da lista
  historico.value.unshift({
    id: proximoIdMovimento++,
    tipo: tipo,
    produto: nomeProduto,
    quantidade: quantidade,
    observacao: observacao || '',
    data: new Date().toLocaleString('pt-BR')
  })
}

// Evento "adicionar" vindo do FormularioProduto
function adicionarProduto(novoProduto) {
  produtos.value.push({ id: proximoIdProduto++, ...novoProduto })
  registrarHistorico('cadastro', novoProduto.nome, novoProduto.quantidade, '')
}

// Evento "remover" vindo do ProdutoCard
function removerProduto(id) {
  const produto = produtos.value.find((item) => item.id === id)
  produtos.value = produtos.value.filter((item) => item.id !== id)
  registrarHistorico('remocao', produto.nome, produto.quantidade, '')
}

// Evento "movimentar" vindo do ProdutoCard: guarda qual produto/tipo
// na variável reativa. Como o modal usa v-if, ele aparece sozinho.
function abrirMovimento(id, tipo) {
  movimentoEmAberto.value = { id: id, tipo: tipo }
}

function produtoDoMovimento() {
  return produtos.value.find((item) => item.id === movimentoEmAberto.value.id)
}

function fecharMovimento() {
  movimentoEmAberto.value = null
}

// Evento "confirmar" vindo do ModalMovimento
function confirmarMovimento(dados) {
  const produto = produtoDoMovimento()
  const tipo = movimentoEmAberto.value.tipo

  if (tipo === 'entrada') {
    produto.quantidade += dados.quantidade
  } else {
    produto.quantidade -= dados.quantidade
  }

  registrarHistorico(tipo, produto.nome, dados.quantidade, dados.observacao)
  fecharMovimento()
}
</script>

<template>
  <AppHeader
    titulo="Estoque Fácil"
    subtitulo="Controle de produtos, entradas, saídas e alertas de estoque baixo"
  />

  <main class="container conteudo">
    <!-- REÚSO: o mesmo componente ResumoEstoque, com props diferentes -->
    <section class="resumos">
      <ResumoEstoque titulo="Produtos cadastrados" :valor="produtos.length" icone="📦" />
      <ResumoEstoque titulo="Unidades em estoque" :valor="totalUnidades()" icone="🏪" />
      <ResumoEstoque titulo="Valor estimado" :valor="valorTotalEstoque()" icone="💰" />
      <ResumoEstoque
        titulo="Produtos em alerta"
        :valor="produtosEmAlerta().length"
        icone="⚠️"
        :destaque="produtosEmAlerta().length > 0"
      />
    </section>

    <section class="painel">
      <FormularioProduto :categorias="listaCategorias()" @adicionar="adicionarProduto" />

      <div class="alertas">
        <p class="eyebrow">Atenção</p>
        <h2>Alertas de estoque</h2>

        <ul v-if="produtosEmAlerta().length > 0" class="alertas__lista">
          <li v-for="produto in produtosEmAlerta()" :key="produto.id">
            <strong>{{ produto.nome }}</strong>
            <span>
              {{ situacaoEstoque(produto) === 'zerado' ? 'sem estoque' : 'restam ' + produto.quantidade + ' (mínimo ' + produto.estoqueMinimo + ')' }}
            </span>
          </li>
        </ul>
        <p v-else class="vazio">Nenhum produto em alerta no momento.</p>
      </div>
    </section>

    <section class="lista">
      <div class="lista__topo">
        <div>
          <p class="eyebrow">Inventário</p>
          <h2>Produtos em estoque</h2>
        </div>

        <div class="filtros">
          <input v-model="busca" type="search" placeholder="Pesquisar por nome..." aria-label="Pesquisar produtos" />

          <select v-model="categoriaFiltro" aria-label="Filtrar por categoria">
            <option value="">Todas as categorias</option>
            <option v-for="categoria in listaCategorias()" :key="categoria" :value="categoria">
              {{ categoria }}
            </option>
          </select>

          <button type="button" class="filtros__limpar" @click="limparFiltros">Limpar</button>
        </div>
      </div>

      <!-- v-if / v-else-if / v-else escolhem o que aparece. REÚSO: um ProdutoCard por produto -->
      <p v-if="produtos.length === 0" class="vazio">Nenhum produto cadastrado ainda.</p>
      <p v-else-if="produtosFiltrados().length === 0" class="vazio">
        Nenhum produto encontrado com esses filtros.
      </p>

      <div v-else class="lista__cards">
        <ProdutoCard
          v-for="produto in produtosFiltrados()"
          :key="produto.id"
          :produto="produto"
          :situacao="situacaoEstoque(produto)"
          @movimentar="abrirMovimento"
          @remover="removerProduto"
        />
      </div>
    </section>

    <section class="historico">
      <p class="eyebrow">Registro</p>
      <h2>Últimas movimentações</h2>

      <ul v-if="historico.length > 0" class="historico__lista">
        <li v-for="movimento in historico.slice(0, 8)" :key="movimento.id">
          <span :class="['historico__tipo', 'historico__tipo--' + movimento.tipo]">
            {{ rotuloMovimento(movimento.tipo) }}
          </span>
          <div class="historico__info">
            <strong>{{ movimento.produto }}</strong> — {{ movimento.quantidade }} un.
            <small>{{ movimento.data }}</small>
            <em v-if="movimento.observacao">“{{ movimento.observacao }}”</em>
          </div>
        </li>
      </ul>
      <p v-else class="vazio">Nenhuma movimentação registrada ainda.</p>
    </section>
  </main>

  <!-- v-if: o modal só existe enquanto movimentoEmAberto não for null -->
  <ModalMovimento
    v-if="movimentoEmAberto"
    :produto="produtoDoMovimento()"
    :tipo="movimentoEmAberto.tipo"
    @confirmar="confirmarMovimento"
    @cancelar="fecharMovimento"
  />

  <AppFooter />
</template>

<style scoped>
.conteudo {
  flex: 1;
  padding-top: var(--space-6);
  padding-bottom: var(--space-7);
  display: flex;
  flex-direction: column;
  gap: var(--space-6);
}

.eyebrow {
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-size: 0.72rem;
  color: var(--color-primary);
  font-weight: 700;
  margin-bottom: 4px;
}

h2 {
  font-size: 1.4rem;
  color: var(--color-text);
}

.vazio {
  color: var(--color-text-muted);
  font-style: italic;
  margin-top: var(--space-3);
}

/* Cards de resumo */
.resumos {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: var(--space-4);
}

/* Formulário + alertas lado a lado */
.painel {
  display: grid;
  grid-template-columns: 3fr 2fr;
  gap: var(--space-4);
  align-items: start;
}

.alertas,
.historico {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--space-5);
  box-shadow: var(--shadow-soft);
}

.alertas__lista,
.historico__lista {
  list-style: none;
  margin-top: var(--space-4);
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}

.alertas__lista li {
  display: flex;
  justify-content: space-between;
  gap: var(--space-3);
  padding: 10px 12px;
  background: #fffafa;
  border: 1px solid #f1c7c3;
  border-radius: var(--radius-sm);
  font-size: 0.9rem;
}

.alertas__lista span {
  color: var(--color-danger);
  font-weight: 600;
}

/* Lista de produtos */
.lista__topo {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  gap: var(--space-4);
  flex-wrap: wrap;
  margin-bottom: var(--space-4);
}

.filtros {
  display: flex;
  gap: var(--space-2);
  flex-wrap: wrap;
}

.filtros input,
.filtros select {
  min-height: 42px;
  padding: 8px 12px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-surface);
  color: var(--color-text);
}

.filtros__limpar {
  min-height: 42px;
  padding: 8px 14px;
  border-radius: var(--radius-sm);
  background: #e2e8f0;
  color: var(--color-text);
  font-weight: 700;
}

.lista__cards {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: var(--space-4);
}

/* Histórico */
.historico__lista li {
  display: flex;
  align-items: flex-start;
  gap: var(--space-3);
  padding: 10px 12px;
  background: var(--color-surface-muted);
  border-radius: var(--radius-sm);
  font-size: 0.9rem;
}

.historico__info small {
  display: block;
  color: var(--color-text-muted);
  font-size: 0.75rem;
}

.historico__info em {
  display: block;
  color: var(--color-text-soft);
  font-size: 0.82rem;
}

.historico__tipo {
  flex: 0 0 76px;
  text-align: center;
  font-size: 0.7rem;
  font-weight: 700;
  padding: 5px 8px;
  border-radius: 999px;
  background: #e2e8f0;
  color: var(--color-text);
}

.historico__tipo--entrada {
  background: #dbeafe;
  color: #1d4ed8;
}

.historico__tipo--saida {
  background: #fef3c7;
  color: #92400e;
}

.historico__tipo--cadastro {
  background: var(--color-primary-soft);
  color: var(--color-primary);
}

.historico__tipo--remocao {
  background: var(--color-danger-soft);
  color: var(--color-danger);
}

/* Responsividade */
@media (max-width: 900px) {
  .resumos {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .painel,
  .lista__cards {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 560px) {
  .resumos {
    grid-template-columns: 1fr;
  }

  .filtros,
  .filtros input,
  .filtros select {
    width: 100%;
  }
}
</style>
