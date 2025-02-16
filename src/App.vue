<template>
  <div id="app">
    <h1>Mi To-Do List en Vue</h1>
    <ToDoList />
  </div>
</template>

<script>
import ToDoList from './components/ToDoList.vue'; // Asegúrate de que este es el camino correcto

export default {
  components: {
    ToDoList
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 50px;
}
</style>
