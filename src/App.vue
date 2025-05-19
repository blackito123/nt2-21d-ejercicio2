<script setup>
import { ref, computed } from 'vue';

// Datos de ejemplo (reemplaza con los datos reales si están en otro componente o archivo)
const personas = ref([
  { nombre: 'Juan Perez', dni: '12345678' },
  { nombre: 'Ana Gomez', dni: '87654321' },
  { nombre: 'Carlos Lopez', dni: '11223344' },
  { nombre: 'Maria Rodriguez', dni: '44332211' },
]);

// Variables reactivas para los filtros
const filtroNombre = ref('');
const filtroDni = ref('');

// Propiedad computada para filtrar personas
const personasFiltradas = computed(() => {
  let resultado = personas.value;

  // Filtro por nombre (mínimo 3 caracteres)
  if (filtroNombre.value.length >= 3) {
    resultado = resultado.filter((persona) =>
      persona.nombre.toLowerCase().includes(filtroNombre.value.toLowerCase())
    );
  }

  // Filtro por DNI (mínimo 3 caracteres)
  if (filtroDni.value.length >= 3) {
    resultado = resultado.filter((persona) =>
      persona.dni.includes(filtroDni.value)
    );
  }

  return resultado;
});

// Propiedad computada para mostrar el alert
const mostrarAdvertencia = computed(() => {
  return (
    (filtroNombre.value.length > 0 && filtroNombre.value.length < 3) ||
    (filtroDni.value.length > 0 && filtroDni.value.length < 3)
  );
});

// Métodos para limpiar filtros
const limpiarFiltroDniSiNecesario = () => {
  if (filtroNombre.value.length === 0 && filtroDni.value.length < 3) {
    filtroDni.value = '';
  }
};

const limpiarFiltroNombreSiNecesario = () => {
  if (filtroDni.value.length === 0 && filtroNombre.value.length < 3) {
    filtroNombre.value = '';
  }
};
</script>

<template>
  <div class="container mt-4">
    <h1 class="mb-4">Lista de Personas</h1>

    <!-- Filtros de búsqueda -->
    <div class="row mb-3">
      <div class="col-md-6">
        <label for="filtroNombre" class="form-label">Filtrar por Nombre:</label>
        <input
          id="filtroNombre"
          v-model="filtroNombre"
          class="form-control"
          placeholder="Ingrese nombre o apellido"
          @input="limpiarFiltroDniSiNecesario"
        />
      </div>
      <div class="col-md-6">
        <label for="filtroDni" class="form-label">Filtrar por DNI:</label>
        <input
          id="filtroDni"
          v-model="filtroDni"
          class="form-control"
          placeholder="Ingrese DNI"
          @input="limpiarFiltroNombreSiNecesario"
        />
      </div>
    </div>

    <!-- Alert de advertencia -->
    <div v-if="mostrarAdvertencia" class="alert alert-warning" role="alert">
      Por favor, ingrese al menos 3 caracteres en alguno de los filtros.
    </div>

    <!-- Lista de personas -->
    <ul class="list-group">
      <li
        v-for="persona in personasFiltradas"
        :key="persona.dni"
        class="list-group-item"
      >
        {{ persona.nombre }} - {{ persona.dni }}
      </li>
      <li v-if="personasFiltradas.length === 0" class="list-group-item text-muted">
        No se encontraron personas que coincidan con los filtros.
      </li>
    </ul>
  </div>
</template>

<style scoped>
.container {
  max-width: 800px;
}
</style>