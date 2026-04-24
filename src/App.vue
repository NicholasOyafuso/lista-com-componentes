<script setup>
import { computed, ref } from 'vue'
import TarefaForm from './components/TarefaForm.vue'
import TarefaFiltro from './components/TarefaFiltro.vue'
import TarefaLista from './components/TarefaLista.vue'
import TarefaConcluidas from './components/TarefaConcluidas.vue'

const tarefas = ref([
  { id: 1, desc: 'fazer nada', status: 'pendente' },
  { id: 2, desc: 'aula jatozzord', status: 'concluida' },
  { id: 3, desc: 'projeto CR7', status: 'pendente' },
])

const filtro = ref('')
const editandoId = ref(null)
const ordenarStatus = ref(false)

const acharI = (id) => tarefas.value.findIndex((x) => x.id === id)

const gerarID = () => Math.max(...tarefas.value.map((t) => t.id), 0) + 1


const tarefasFiltradas = computed(() => {
  if (!filtro.value.trim()) return tarefas.value
  return tarefas.value.filter((t) =>
    t.desc.toLowerCase().includes(filtro.value.toLowerCase()),
  )
})

const contagemProgresso = computed(() => {
  const concluidas = tarefas.value.filter((t) => t.status === 'concluida').length
  const pendentes = tarefas.value.length - concluidas
  return { concluidas, pendentes }
})

const tarefaEmEdicao = computed(() => {
  if (editandoId.value === null) return null
  return tarefas.value.find((t) => t.id === editandoId.value) ?? null
})


const adicionarTarefa = (desc) => {
  tarefas.value.push({ id: gerarID(), desc, status: 'pendente' })
}

const salvarEdicao = (desc) => {
  const i = acharI(editandoId.value)
  if (i !== -1) tarefas.value[i].desc = desc
  editandoId.value = null
}

const cancelarEdicao = () => {
  editandoId.value = null
}

const remover = (id) => {
  const i = acharI(id)
  if (i >= 0) tarefas.value.splice(i, 1)
}

const iniciarEdicao = (id) => {
  editandoId.value = id
}

const concluir = (id) => {
  const i = acharI(id)
  if (i === -1) return
  tarefas.value[i].status =
    tarefas.value[i].status === 'pendente' ? 'concluida' : 'pendente'
}

const ordenar = () => {
  ordenarStatus.value = !ordenarStatus.value
  tarefas.value.sort(
    ordenarStatus.value
      ? (a, b) => a.desc.localeCompare(b.desc)
      : (a, b) => a.id - b.id,
  )
}
</script>







<template>
  <div class="container">
    <TarefaConcluidas v-bind="contagemProgresso" />

    <TarefaForm
      :editando="tarefaEmEdicao"
      @adicionar="adicionarTarefa"
      @salvar="salvarEdicao"
      @cancelar="cancelarEdicao"
    />

    <TarefaFiltro
      v-model="filtro"
      :pode-ordenar="tarefas.length > 1"
      :ordenado="ordenarStatus"
      @ordenar="ordenar"
    />

    <TarefaLista
      :tarefas="tarefasFiltradas"
      :editando-id="editandoId"
      @concluir="concluir"
      @remover="remover"
      @editar="iniciarEdicao"
    />
  </div>
</template>










<style scoped>
.container {
  max-width: 500px;
  margin: 40px auto;
  padding: 24px;
  background: #dda0dd;
  border-radius: 8px;
}
</style>
