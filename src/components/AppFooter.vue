<script setup>
defineProps({
  ano: {
    type: [String, Number],
    default: () => new Date().getFullYear()
  }
})
</script>

<template>
  <footer class="app-footer">
    <div class="container">
      <p>Estoque Fácil © {{ ano }} — Projeto acadêmico de gestão de estoques.</p>
    </div>
  </footer>
</template>

<style scoped>
.app-footer {
  margin-top: auto;
  background: #0f172a;
  color: #cbd5e1;
  text-align: center;
  padding: 18px 0;
  font-size: 0.8rem;
  border-top: 1px solid rgba(148, 163, 184, 0.2);
}
</style>
