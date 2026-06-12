<script setup lang="ts">
import ClickCounter from './components/ClickCounter.vue'
import ItemList from './components/ItemList.vue'
import TodoList from './components/TodoList.vue'
import WelcomeMessage from './components/WelcomeMessage.vue'
</script>

<template>
  <main>
    <WelcomeMessage msg="Webエンジニアになろう講習会へようこそ" />
    <ClickCounter />
    <ItemList />
    <TodoList />
  </main>
</template>

<style scoped>
main {
  padding: 2rem;
}
</style>
