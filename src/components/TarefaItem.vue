<script setup>
defineProps({
  tarefa: { type: Object, required: true },

})

const emit = defineEmits(['concluir', 'remover', 'editar'])
</script>

<template>
  <li>
    <div
      class="tarefa"
      :class="{ concluido: tarefa.status === 'concluida' }"
      @click="emit('concluir', tarefa.id)"
    >
      {{ tarefa.desc }}
    </div>

    <div class="acoes">
      <button  @click="emit('remover', tarefa.id)">
        Remover
      </button>
      <button  @click="emit('editar', tarefa.id)">
        Editar
      </button>
    </div>
  </li>
</template>

<style scoped>
li {
  background: white;
  padding: 10px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  list-style: none;
}

.tarefa {
  cursor: pointer;
  padding: 5px;
  flex: 1;
}

.concluido {
  color: green;
  text-decoration: line-through;
}

.acoes {
  display: flex;
  gap: 5px;
}

.acoes button {
  padding: 6px 10px;
  border: none;
  background: #ffd700;
  cursor: pointer;
}

</style>
