# Estoque Fácil

Aplicação **One Page** em Vue 3 para controle simples de estoque: cadastro de produtos, entradas e saídas, histórico de movimentações e alertas de estoque baixo. Desenvolvida como atividade acadêmica de Front-end.

## Funcionalidades

- Cadastro de produtos (nome, categoria, quantidade, preço e estoque mínimo).
- Pesquisa por nome e filtro por categoria.
- Registro de entrada e saída de produtos, com observação opcional.
- Edição de produto (nome, preço unitário e estoque mínimo), com validações e mensagem de sucesso.
- Histórico das últimas movimentações, incluindo as edições (com os campos alterados).
- Indicadores (produtos, unidades, valor estimado, produtos em alerta) e painel de alertas.
- Remoção de produto com confirmação.
- Layout responsivo (computador e celular).

Os dados ficam apenas na memória da página: ao recarregar, a lista volta ao estado inicial. O projeto não tem backend, API ou banco de dados.

## Tecnologias e limites da atividade

- Vue 3 com componentes `.vue` e `<script setup>` (Vite para rodar o projeto).
- CSS próprio, sem framework CSS ou biblioteca de interface.
- Página única, sem Vue Router e sem biblioteca de estado.
- Somente conteúdo visto em aula: componentes, props, `ref()`, eventos, `v-model`, `v-if`, `v-show`, `v-for` e CSS.

## Como executar

Requisitos: Node.js e npm.

```bash
npm install
npm run dev
```

Abra o endereço mostrado no terminal (normalmente `http://localhost:5173`).

## Estrutura

```text
src/
  App.vue                     Componente raiz: estado (ref) e ações
  main.js                     Inicialização do Vue
  style.css                   Estilos globais, variáveis e estilos compartilhados dos modais
  components/
    AppHeader.vue             Cabeçalho (props: titulo, subtitulo)
    ResumoEstoque.vue         Indicador reutilizado 4 vezes (props: titulo, valor, icone, destaque)
    FormularioProduto.vue     Formulário de cadastro (prop: categorias; evento: adicionar)
    ProdutoCard.vue           Card de produto, 1 por item da lista (props: produto, situacao)
    ModalMovimento.vue        Janela de entrada/saída (props: produto, tipo; eventos: confirmar, cancelar)
    ModalEditarProduto.vue    Janela de edição (prop: produto; eventos: salvar, cancelar)
    AppFooter.vue             Rodapé (prop: ano)
```

## Onde estão os conceitos

| Conceito | Onde ver |
|---|---|
| `ref()` | `App.vue`: `produtos`, `historico`, `busca`, `categoriaFiltro`, `movimentoEmAberto`, `produtoEmEdicao`, `mensagem` |
| Props | `ProdutoCard.vue` (`produto`, `situacao`), enviadas pelo `App.vue` dentro do `v-for` |
| Eventos | `@click` em `ProdutoCard.vue`; `@submit.prevent` em `FormularioProduto.vue`; `emit` nos componentes filhos |
| `v-model` | campos do formulário, pesquisa, filtro de categoria e modal |
| `v-if` / `v-else` | modais de movimentação e de edição, mensagem de sucesso, mensagens de erro, listas vazias e alertas |
| `v-show` | selo "Estoque baixo / Sem estoque" em `ProdutoCard.vue` |
| `v-for` | cards de produto, indicadores, alertas, categorias e histórico |
| Reúso | `ResumoEstoque` (4 usos com dados diferentes) e `ProdutoCard` (um por produto) |

**Exemplo de reatividade:** clicar em "Entrada" ou "Saída" dispara `@click` no `ProdutoCard`, que emite `movimentar`. O `App.vue` guarda o produto e o tipo em `movimentoEmAberto` (um `ref`). Como o `ModalMovimento` usa `v-if="movimentoEmAberto"`, ele aparece na tela. Ao confirmar, `produto.quantidade` muda e a interface atualiza os cards, os indicadores e os alertas.

## Prompts de IA utilizados

Resumo das solicitações feitas durante o desenvolvimento:

1. **Planejamento dos componentes** 
   > "Tenho uma aplicação Vue sobre estoque. Sugira uma divisão em componentes simples e reutilizáveis. Utilize apenas Vue com `<script setup>`, sem bibliotecas externas e sem Vue Router."

2. **Criação do card de resumo reutilizável**
   > "Crie um componente Vue simples que receba título e descrição através de props. Utilize `<script setup>` e CSS próprio, sem bibliotecas externas."

3. **Reúso de componente com props diferentes** (etapa de integração no App.vue):
   > "Como posso reutilizar este componente para apresentar diferentes informações usando props?"

4. **Formulário de cadastro** (etapa de interação/reatividade):
   > "Como utilizar v-model neste formulário Vue? Explique o funcionamento do código."

5. **Lista de produtos** 
   > "Como utilizar v-for para apresentar esta lista utilizando um componente reutilizável?"

1. **Revisão visual e funcional:**
   > "Analise o Estoque Fácil existente e aprimore dashboard, filtros, ações, relatórios, cálculos, responsividade e acessibilidade. Preserve a arquitetura e os dados existentes e valide o resultado."
   
3. **Histórico de movimentações:**
   > "Investigue por que a observação preenchida no modal não aparece nas movimentações recentes. Verifique o fluxo de salvamento e exibição, corrija a interface e mantenha compatibilidade com movimentações antigas sem observação."
   
5. **Adequação ao conteúdo da disciplina:**
   > "Simplifique o App.vue para usar somente o conteúdo visto em aula (componentes, props, ref, eventos, v-model, v-if, v-show, v-for e CSS), mantendo cadastro, filtros, movimentação e alertas."
7. **Edição de produto:**
   >"Adicione uma funcionalidade de edição de produto ao sistema de estoque: botão Editar em cada card, modal com o mesmo visual dos modais de entrada e saída (nome, preço e estoque mínimo), validações, mensagem de sucesso e registro da edição no histórico com os campos alterados."
9. **Documentação acadêmica:**
    > "Atualize o README para descrever o projeto real, os componentes Vue, os conceitos demonstráveis, os prompts utilizados e as instruções de execução."

A IA foi usada como apoio. Todo o código foi revisado, testado e compreendido pela equipe antes da entrega.

## Repositório e equipe

- **Repositório público:** https://github.com/alexssandrosouza020-ui/Estoque-Facil
  
- **Integrantes:**
- Alexssandro Souza de Oliveira - 04206917
- Alyson Raphael Cruz Santos - 04213803
- Erielson da Silva Teles - 04211403
- Thiago Viana Tenório - 04211037
- João Victor Bentes - 04211861
- Kevyn Bastos Fernandes - 04214212
- Benedito Graziel de Souza Farias - 04208454

  
- **Vídeo de apresentação:** (inserir o link)
