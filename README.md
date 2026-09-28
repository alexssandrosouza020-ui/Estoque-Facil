# Estoque Fácil

Aplicação **One Page** em Vue.js para controle simples de produtos em estoque, desenvolvida como atividade da disciplina.

## Como executar

```bash
npm install
npm run dev
```

Depois abra o endereço mostrado no terminal (normalmente `http://localhost:5173`).

## Tema

Estoque — cadastro e visualização de produtos, quantidades e valores.

## Estrutura do projeto

```
src/
  App.vue                       -> componente raiz, guarda o estado e a lógica principal
  components/
    AppHeader.vue                -> cabeçalho da aplicação
    ResumoEstoque.vue             -> card de estatística (reutilizado 3x)
    FormularioProduto.vue         -> formulário de cadastro de produtos
    ProdutoCard.vue               -> card de um produto (reutilizado via v-for)
    AppFooter.vue                 -> rodapé da aplicação
```

## Requisitos técnicos atendidos

- Vue.js, aplicação One Page, sem Vue Router
- Sem bibliotecas externas de UI/CSS
- 5 componentes `.vue` com `<script setup>`, usados em `App.vue`
- Reatividade com `ref()` e `computed()` (lista de produtos, totais)
- Eventos: `@click` (remover produto), `@submit.prevent` (cadastrar produto)
- Diretivas: `v-if`, `v-show`, `v-for`, `v-model`
- Props: `titulo`, `subtitulo`, `produto`, `valor`, `icone`, `destaque`, `ano`
- Reúso de componentes: `ResumoEstoque` (3 usos) e `ProdutoCard` (1 por produto)
- CSS próprio (scoped) em cada componente

## Uso de Inteligência Artificial

A IA (Claude) foi utilizada como apoio para estruturar e revisar o código. Os principais prompts utilizados foram:

1. **Planejamento dos componentes** (início do desenvolvimento):
   > "Tenho uma aplicação Vue sobre estoque. Sugira uma divisão em componentes simples e reutilizáveis. Utilize apenas Vue com `<script setup>`, sem bibliotecas externas e sem Vue Router."

2. **Criação do card de resumo reutilizável** (etapa de componentização):
   > "Crie um componente Vue simples que receba título e descrição através de props. Utilize `<script setup>` e CSS próprio, sem bibliotecas externas."

3. **Reúso de componente com props diferentes** (etapa de integração no App.vue):
   > "Como posso reutilizar este componente para apresentar diferentes informações usando props?"

4. **Formulário de cadastro** (etapa de interação/reatividade):
   > "Como utilizar v-model neste formulário Vue? Explique o funcionamento do código."

5. **Lista de produtos** (etapa de listagem):
   > "Como utilizar v-for para apresentar esta lista utilizando um componente reutilizável?"

6. **Revisão final** (antes da entrega):
   > "Revise este código utilizando somente os conceitos básicos de Vue: componentes, props, ref, eventos, v-if, v-for e v-model. Não utilize bibliotecas externas nem conceitos avançados."

Todo o código gerado foi revisado e compreendido pela equipe antes da entrega.

## Equipe

- (preencher com os nomes dos integrantes)
