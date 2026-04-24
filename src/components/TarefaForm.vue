<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  editando: { type: Object, default: null },
})

const emit = defineEmits(['adicionar', 'salvar', 'cancelar'])

const texto = ref('')

watch(
  () => props.editando,
  (tarefa) => {
    texto.value = tarefa ? tarefa.desc : ''
  },
)

const confirmar = () => {
  if (!texto.value.trim()) {
    alert('Escreva algo na caixa de texto')
    return
  }

  if (props.editando) {
    emit('salvar', texto.value)
  } else {
    emit('adicionar', texto.value)
  }

  texto.value = ''
}

const cancelar = () => {
  texto.value = ''
  emit('cancelar')
}
</script>

<template>
  <div class="form">
    <input
      type="text"
      placeholder="Digite sua tarefa"
      v-model="texto"
      @keyup.enter="confirmar"
    />

    <button @click="confirmar">
      {{ editando ? 'Salvar' : 'Adicionar' }}
    </button>

    <button v-if="editando" class="btn-cancelar" @click="cancelar">
      Cancelar
    </button>
  </div>
</template>

<style scoped>
.form {
  display: flex;
  gap: 6px;
  margin-bottom: 10px;
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

.btn-cancelar {
  background: #e57373;
}
</style>
