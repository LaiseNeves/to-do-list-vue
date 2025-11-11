<script setup>
import { ref, watch, onMounted } from 'vue'

// lista de tarefas
const tasks = ref([])

// texto digitado
const newTask = ref('')

// Para carregar tarefas ao iniciar
onMounted(() => {
  const saved = localStorage.getItem('tasks')
  if (saved) {
    tasks.value = JSON.parse(saved)
  }
})

// Para sempre que tasks mudar ---> salva no localStorage
watch(
  tasks,
  (newValue) => {
    localStorage.setItem('tasks', JSON.stringify(newValue))
  },
  { deep: true } // importante para detectar mudanças dentro do objeto
)

// função para adicionar tarefa
function addTask() {
  if (newTask.value.trim() === '') return

  tasks.value.push({
    id: Date.now(),
    text: newTask.value,
    done: false
  })

  newTask.value = ''
}

// marcar como feito
function toggleDone(task) {
  task.done = !task.done
}

// remover tarefa
function removeTask(id) {
  tasks.value = tasks.value.filter(task => task.id !== id)
}
</script>

<template>
  <h1>To Do List com Vue.js</h1>

  <div class="input-area">
    <input 
      type="text" 
      v-model="newTask" 
      placeholder="Digite uma tarefa..."
    />
    <button @click="addTask">Adicionar</button>
  </div>

  <ul>
    <li v-for="task in tasks" :key="task.id">
      <span 
        :class="{ done: task.done }"
        @click="toggleDone(task)"
      >
        {{ task.text }}
      </span>
      <button class="remove" @click="removeTask(task.id)">x</button>
    </li>
  </ul>
</template>

<style>
.input-area {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 20px;
}

input {
  background-color: white; /* cor da caixa "digite uma tarefa" para branca */
  color: #4a0066; /* cor do texto digitado  */
  border: 2px solid #101011; /* borda da caixa */
  padding: 10px;
  border-radius: 6px;
  width: 300px;
}


input::placeholder {
  color: #6e4fb0;
}

button {
  padding: 10px 14px;
  background: #6e278b;
  border: none;
  color: white;
  font-weight: bold;
  border-radius: 6px;
  cursor: pointer;
  transition: 0.2s;
}

button:hover {
  background: #6e4fb0;
}


ul {
  list-style: none;
  padding: 0;
  max-width: 400px;
  margin: auto;
}

li {
  background: white;
  padding: 10px;
  margin-bottom: 8px;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.done {
  text-decoration: line-through;
  opacity: 0.6;
}

.remove {
  background: red;
  color: white;
  border: none;
  padding: 4px 8px;
  border-radius: 3px;
}
</style>


<style>
body {
  font-family: Arial, sans-serif;
  background: #f4f4f4;
  margin: 0;
  padding: 20px;
}
h1 {
  text-align: center;
}
</style>
