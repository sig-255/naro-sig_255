<script setup lang="ts">
import { ref } from 'vue'
interface Todo {
  done: boolean
  name: string
  addAt: Date
  doneAt?: Date
}
const doneList = ref<Todo[]>([
  { done: true, name: '今日の講習会に参加する', addAt: new Date(Date.now() - 4000 * 1000), doneAt: new Date(Date.now())}
])
const yetList = ref<Todo[]>([
  { done: false, name: 'Vueを理解する', addAt: new Date(Date.now() - 4000 * 1000)}
])

const newTodoName = ref('')
const addTodo = () => {
  if (yetList.value.some((todo) => todo.name === newTodoName.value)) {
    return
  }
  if (doneList.value.some((todo) => todo.name === newTodoName.value)) {
    return
  }
  if (newTodoName.value === '') {
    return
  }
  yetList.value.push({ done: false, name: newTodoName.value, addAt: new Date(Date.now())})
  newTodoName.value = ''
}
const doneTodo = (name: string) => {
  const todo = yetList.value.find(todo => todo.name === name)
  if (!todo) {
    return
  }
  todo.done = true
  todo.doneAt = new Date(Date.now())
  doneList.value.push(todo)
  yetList.value = yetList.value.filter(todo => todo.name !== name)
}
const undoTodo = (name: string) => {
  const todo = doneList.value.find(todo => todo.name === name)
  if (!todo) {
    return
  }
  todo.done = false
  todo.doneAt = undefined
  todo.addAt = new Date(Date.now())
  yetList.value.push(todo)
  doneList.value = doneList.value.filter(todo => todo.name !== name)
}
const deleteTodo = (name: string) => {
  yetList.value = yetList.value.filter(todo => todo.name !== name)
  doneList.value = doneList.value.filter(todo => todo.name !== name)
}
</script>

<template>
  <details open><summary>Todolist</summary>
    <h3>未完</h3>
    <ul>
      <li v-for="todo in yetList" :key="todo.name">
        <div>{{ todo.name }}</div>
        <div>追加日時: {{ todo.addAt.toLocaleString() }}</div>
        <button @click="deleteTodo(todo.name)">削除</button>
        <button @click="doneTodo(todo.name)">完了へ</button>
      </li>
    </ul>
    <h3>完了</h3>
    <ul>
      <li v-for="todo in doneList" :key="todo.name">
        <div>{{ todo.name }}</div>
        <div>追加日時: {{ todo.addAt.toLocaleString() }}</div>
        <div>完了日時: {{ todo.doneAt?.toLocaleString() }}</div>
        <button @click="deleteTodo(todo.name)">削除</button>
        <button @click="undoTodo(todo.name)">未完へ</button>
      </li>
    </ul>
    <div>
      <input v-model="newTodoName" type="text" />
      <button @click="addTodo()">追加</button>
    </div>
  </details>
</template>