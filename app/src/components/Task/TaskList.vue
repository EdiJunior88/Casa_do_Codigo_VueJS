<script setup lang="js">
import { computed } from 'vue'

// A função defineProps é usada para definir as
// propriedades esperadas no componente. Neste caso,
// espera-se uma propriedade chamada todoList do tipo Array
const props = defineProps({
  todoList: Array
})

// Aqui, computed é usada para criar uma propriedade computada
// chamada sortedTasks. Esta propriedade retorna o array todoList
// ordenado alfabeticamente por título.
const sortedTasks = computed(() => {
  // Verifica se a propriedade todoList existe e se é um array,
  // Caso não exista ou seja um array vazio, retorna uma lista vazia
  if (!props.todoList || !Array.isArray(props.todoList)) {
    return []
  }

  // A função recebe uma lista (props.todoList) de tarefas
  // e ordena alfabeticamente por título (title) de cada objeto na lista
  const sorted = props.todoList
  return sorted.sort(function (a, b) {
    if (a.title < b.title) return -1
    if (a.title > b.title) return 1
    return 0
  })
})

// A função recebe uma tarefa (task) e a altera de acordo com o argumento
// completando ou não a tarefa (alternando o estado)
function completeTasks(task) {
  task.completed = !task.completed
}
</script>

<template>
  <ul class="list-none -mt-1.5 p-0">
    <TransitionGroup name="list">
      <li
        v-for="todo in sortedTasks"
        :key="todo"
        class="w-full relative m-0 -mt-1 text-2xl font-normal bg-white text-violet-900 py-4 border border-solid border-gray-200 first:border-t-0 last:border-b-0 shadow-lg rounded-b-md focus:outline-gray-100 focus:ring focus:ring-gray-100"
      >
        <div class="flex items-center">
          <input
            type="checkbox"
            class="toggle ml-4 mr-3 border border-solid border-gray-500"
            @click="completeTasks(todo)"
          />
          <label :class="{ 'todo-completed': todo.completed }">{{ todo.title }}</label>
        </div>
      </li>
    </TransitionGroup>
  </ul>
</template>

<style lang="css" scoped>
.toggle {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  outline: none;
  cursor: pointer;
  position: relative;
}

.toggle::before {
  content: '✔️';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 14px;
  color: transparent;
  text-shadow: 0 0 0 rgb(11, 165, 19);
  visibility: hidden;
}

.toggle:checked::before {
  visibility: visible;
}

.todo-completed {
  text-decoration: line-through;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
