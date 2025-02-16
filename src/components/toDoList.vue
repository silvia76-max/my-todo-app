<template>
  <div>
    <h2>Lista de Tareas</h2>
    
    <input v-model="nuevaTarea" @keyup.enter="agregarTarea" placeholder="Añadir tarea" />
    <button @click="agregarTarea">Agregar</button>
    
      <ul>
  <li v-for="(tarea, index) in tareas" :key="index" :class="{ completada: tarea.completada }">
    <span v-if="!tarea.editando" @click="toggleCompletada(index)">{{ tarea.texto }}</span>
    <input v-else v-model="tarea.texto" @keyup.enter="guardarEdicion(index)" @blur="guardarEdicion(index)" />
    
    <button @click="editarTarea(index)">✏️</button>
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
        this.tareas.push({ texto: this.nuevaTarea, completada: false });
        this.nuevaTarea = "";
        this.guardarEnLocalStorage();
      }
        },
        editarTarea(index) {
      this.tareas[index].editando = true;
    },
      guardarEdicion(index) {
      this.tareas[index].editando = false;
      this.guardarEnLocalStorage(); // Guardamos el cambio en localStorage
      }
      agregarTarea() {
    if (this.nuevaTarea.trim() !== "") {
      this.tareas.push({ texto: this.nuevaTarea, completada: false, editando: false });
      this.nuevaTarea = "";
      this.guardarEnLocalStorage();
    }
  }
    eliminarTarea(index) {
      this.tareas.splice(index, 1);
      this.guardarEnLocalStorage();
    },
    toggleCompletada(index) {
      this.tareas[index].completada = !this.tareas[index].completada;
      this.guardarEnLocalStorage();
    },
    guardarEnLocalStorage() {
      localStorage.setItem("tareas", JSON.stringify(this.tareas));
    },
    cargarDesdeLocalStorage() {
      const tareasGuardadas = localStorage.getItem("tareas");
      if (tareasGuardadas) {
        this.tareas = JSON.parse(tareasGuardadas);
      }
    }
  },
  created() {
    this.cargarDesdeLocalStorage();
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
  margin-left: 5px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 5px 0;
  padding: 5px;
  border-bottom: 1px solid #ccc;
}

.completada {
  text-decoration: line-through;
  color: gray;
}
</style>
