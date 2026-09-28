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
    <p>Estoque Fácil &copy; {{ ano }} — Projeto acadêmico desenvolvido em Vue.js</p>
  </footer>
</template>

<style scoped>
.app-footer {
  margin-top: auto;
  background-color: #1f2933;
  color: #cbd2d9;
  text-align: center;
  padding: 16px 0;
  font-size: 0.85rem;
}
</style>
