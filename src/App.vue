<script setup>
import { computed, ref } from 'vue'

let tarefas = ref([
  { id: 1, desc: 'fazer nada', status: 'pendente' },
  { id: 2, desc: 'aula jatozzord', status: 'concluida' },
  { id: 3, desc: 'projeto CR7', status: 'pendente' },
])
const filtro = ref('')

const tarefasFiltradas = computed(() => {
  if (filtro.value.trim().length === 0) return tarefas.value

  return tarefas.value.filter((item) =>
    item.desc.toLowerCase().includes(filtro.value.toLowerCase()),
  )
})
const textoDoInput = ref('')

const gerarID = () => {
  const id = Math.max(...tarefas.value.map((item) => item.id), 0) + 1
  return id
}
const editandoS = ref(null)
const editar = (id) => {
  const i = acharI(id)
  if (i === -1 || tarefas.value[i].desc === '') return

  textoDoInput.value = tarefas.value[i].desc
  editandoS.value = id
}
const acharI = (id) => {
  return tarefas.value.findIndex((x) => x.id == id)
}

const add = () => {
  if (textoDoInput.value.trim() === '') {
    alert('Escreva algo na caicha de texto')
    return
  }

  if (editandoS.value !== null) {
    const i = acharI(editandoS.value)

    if (i !== -1) {
      tarefas.value[i].desc = textoDoInput.value
    }

    editandoS.value = null
  } else {
    const tarefa = {
      id: gerarID(),
      desc: textoDoInput.value,
      status: 'pendente',
    }
    tarefas.value.push(tarefa)
  }
  textoDoInput.value = ''
}

const remove = (id) => {
  const i = acharI(id)
  if (i >= 0) {
    tarefas.value.splice(i, 1)
  }
}
const contagemProgresso = computed(() => {
  let concluidas = 0
  let pendentes = 0
  tarefas.value.map((item) => {
    if (item.status === 'pendente') {
      pendentes++
    } else {
      concluidas++
    }
  })
  return `Concluidas: ${concluidas}  Pendentes: ${pendentes}`
})

const ordenarStatus = ref(false)
const ordenar = () => {
  ordenarStatus.value = !ordenarStatus.value
  if (ordenarStatus.value) {
    tarefas.value.sort((a, b) => a.desc.localeCompare(b.desc))
  } else {
    tarefas.value.sort((a, b) => a.id - b.id)
  }
}

const concluir = (id) => {
  const i = acharI(id)
  if (i === -1) return
  tarefas.value[i].status = tarefas.value[i].status === 'pendente' ? 'concluida' : 'pendente'
  console.log(tarefas.value[i].status)
}
</script>
<template>
  <div class="container">
    <h1>{{ contagemProgresso }}</h1>
    <input type="text" placeholder="digite sua tarefa" v-model="textoDoInput" @keyup.enter="add" />
    <button @click="add">  {{ editandoS !== null ? 'Salvar' : 'Adicionar' }}</button>
    <button v-if="editandoS !== null" @click="editandoS = null; textoDoInput = ''">Cancelar</button>
<button @click="ordenar" :disabled="tarefas.length <= 1">ordenar</button>
    <div>
      <input type="text" placeholder="filtrar tarefa" v-model="filtro" />
    </div>
    <ul>
      <li v-for="item in tarefasFiltradas" :key="item.id" style="display: flex; ">
        <div
          class="tarefa"
          :class="item.status === 'concluida' ? 'concluido' : ''"
          @click="concluir(item.id)"

        >
          {{ item.desc }}
        </div>
        <div class="btn-fds">
          <button @click="remove(item.id)" :disabled="editandoS !== null">Remover</button>
          <button @click="editar(item.id)" :disabled="editandoS !== null">Editar</button>
        </div>
      </li>
    </ul>

  </div>
</template>

<style scoped>

.concluido {
  color: green;
  text-decoration: line-through;
}
li {
  cursor: pointer;
}
.container {
  max-width: 500px;
  margin: 40px ;
  padding: 20px;
  background: #DDA0DD;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

input {
  width: 70%;
  padding: 10px;
  border: 1px solid #ccc;
  margin-right: 10px;
}

button {
  padding: 10px 12px;
  border: none;
  background: #4caf50;
  color: white;
}

ul {
  padding: 0;
  margin-top: 20px;
}

li {
  background: white;
  padding: 10px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.tarefa {
  cursor: pointer;
  padding: 5px;
}

.concluido {
  text-decoration: line-through;
}

.btn-fds button {
  margin-left: 5px;
  background: #FFD700;
}



</style>
