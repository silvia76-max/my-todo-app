<template>
  <div :class="modoOscuro ? 'dark-mode' : 'light-mode'">
    

    <!-- Botón para cambiar entre modo día/noche -->
    <button @click="toggleModo">🌞/🌙</button>

    <!-- Input para agregar tareas -->
    <input v-model="nuevaTarea" @keyup.enter="agregarTarea" placeholder="Añadir tarea..." />
    <button @click="agregarTarea">Agregar</button>

    <!-- Controles para filtrar y ordenar -->
    <div class="controles">
      <button @click="filtro = 'todas'">Todas</button>
      <button @click="filtro = 'pendientes'">Pendientes</button>
      <button @click="filtro = 'completadas'">Completadas</button>

      <button @click="ordenarPorTexto">Ordenar A-Z</button>
      <button @click="ordenarPorEstado">Ordenar por Estado</button>
    </div>

    <!-- Lista de tareas -->
    <ul>
      <li v-for="(tarea, index) in tareasFiltradas" :key="index">
        <input type="checkbox" v-model="tarea.completada" @change="toggleCompletada(index)" />
        <span :class="{ completada: tarea.completada }">{{ tarea.texto }}</span>

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
      tareas: [],
      filtro: "todas",
      modoOscuro: false, // Estado inicial del modo oscuro
    };
  },
  computed: {
    tareasFiltradas() {
      if (this.filtro === "pendientes") {
        return this.tareas.filter(t => !t.completada);
      } else if (this.filtro === "completadas") {
        return this.tareas.filter(t => t.completada);
      }
      return this.tareas;
    }
  },
  methods: {
    toggleModo() {
      this.modoOscuro = !this.modoOscuro;
      localStorage.setItem("modoOscuro", this.modoOscuro);
    },
    agregarTarea() {
      if (this.nuevaTarea.trim() !== "") {
        this.tareas.push({ texto: this.nuevaTarea, completada: false });
        this.nuevaTarea = "";
        this.guardarEnLocalStorage();
      }
    },
    toggleCompletada(index) {
      this.tareas[index].completada = !this.tareas[index].completada;
      this.guardarEnLocalStorage();
    },
    eliminarTarea(index) {
      this.tareas.splice(index, 1);
      this.guardarEnLocalStorage();
    },
    ordenarPorTexto() {
      this.tareas.sort((a, b) => a.texto.localeCompare(b.texto));
      this.guardarEnLocalStorage();
    },
    ordenarPorEstado() {
      this.tareas.sort((a, b) => a.completada - b.completada);
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
      const modoGuardado = localStorage.getItem("modoOscuro");
      this.modoOscuro = modoGuardado === "true";
    }
  },
  created() {
    this.cargarDesdeLocalStorage();
  }
};
</script>

<style>
/* Estilos generales */
body {
  transition: background 0.3s, color 0.3s;
}

/* Modo claro */
.light-mode {
  background-color: white;
  color: black;
}

/* Modo oscuro */
.dark-mode {
  background-color: #121212;
  color: white;
}

/* Estilos de botones */
button {
  margin: 5px;
  padding: 5px;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  transition: background 0.3s, color 0.3s;
}

/* Botón de modo oscuro */
button:hover {
  background: #555;
  color: white;
}

/* Tareas completadas */
.completada {
  text-decoration: line-through;
  color: gray;
}
</style>
