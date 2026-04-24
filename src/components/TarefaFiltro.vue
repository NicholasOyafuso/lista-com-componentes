<script setup>
defineProps({
  modelValue: { type: String, default: '' },
  podeOrdenar: { type: Boolean, default: true },
  ordenado: { type: Boolean, default: false },
})

const emit = defineEmits(['update:modelValue', 'ordenar'])
</script>

<template>
  <div class="filtro">
    <input
      type="text"
      placeholder="Filtrar tarefa"
      :value="modelValue"
      @input="emit('update:modelValue', $event.target.value)"
    />

    <button :disabled="!podeOrdenar" @click="emit('ordenar')">
      {{ ordenado ? 'Ordenar' : 'Ordenar' }}
    </button>
  </div>
</template>

<style scoped>
.filtro {
  display: flex;
  gap: 6px;
  margin-bottom: 16px;
  flex-wrap: wrap;
}

input {
  flex: 1;
  min-width: 0;
  padding: 10px;
  border: 1px solid #ccc;
}

button {
  padding: 10px 12px;
  border: none;
  background: #4caf50;
  color: white;
  cursor: pointer;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>
