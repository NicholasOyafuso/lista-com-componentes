<script setup>
import TarefaItem from './TarefaItem.vue'

defineProps({
  tarefas: { type: Array, required: true },
  editandoId: { type: Number, default: null },
})

const emit = defineEmits(['concluir', 'remover', 'editar'])
</script>

<template>
  <ul>
    <TarefaItem
      v-for="tarefa in tarefas"
      :key="tarefa.id"
      :tarefa="tarefa"
      :bloqueado="editandoId !== null"
      @concluir="emit('concluir', $event)"
      @remover="emit('remover', $event)"
      @editar="emit('editar', $event)"
    />

    <li v-if="tarefas.length === 0" class="vazio">Nenhuma tarefa encontrada.</li>
  </ul>
</template>

<style scoped>
ul {
  padding: 0;
  margin-top: 10px;
}

.vazio {
  text-align: center;
  color: #555;
  list-style: none;
  padding: 16px;
  background: white;
}
</style>
