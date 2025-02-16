<template>
  <div>
    <h2>Lista de Tareas</h2>
    <input v-model="nuevaTarea" @keyup.enter="agregarTarea" placeholder="Añadir tarea" />
    <button @click="agregarTarea">Agregar</button>
    <ul>
      <li v-for="(tarea, index) in tareas" :key="index">
        {{ tarea }}
        <button @click="eliminarTarea(index)">❌</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nuevaTarea: "",
      tareas: []
    };
  },
  methods: {
    agregarTarea() {
      if (this.nuevaTarea.trim() !== "") {
        this.tareas.push(this.nuevaTarea);
        this.nuevaTarea = "";
      }
    },
    eliminarTarea(index) {
      this.tareas.splice(index, 1);
    }
  }
};
</script>

<style>
input {
  padding: 5px;
  margin-right: 10px;
}
button {
  cursor: pointer;
}
</style>
