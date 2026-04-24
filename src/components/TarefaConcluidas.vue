<script setup>
defineProps({
  concluidas: { type: Number, required: true },
  pendentes: { type: Number, required: true },
})
</script>

<template>
  <h1>Concluídas: {{ concluidas }} &nbsp; Pendentes: {{ pendentes }}</h1>
</template>

<style scoped>
h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
  font-size: 1.1rem;
}
</style>
