<template>
    <v-container width="1200px">
      <!-- Mostrar <v-alert> en caso de error -->
      <v-alert
        v-if="error"
        type="error"
        prominent
        border="center"
        color="red"
        class="my-4"
      >
        Error al obtener datos de la tabla
      </v-alert>
  
      <!-- Mostrar <v-progress-circular> si loading está activo y no hay error -->
      <v-progress-circular
        v-if="loading && !error"
        indeterminate
        color="white"
        size="50"
        class="mx-auto my-5"
      >
      </v-progress-circular>
  
      <!-- Mostrar el nombre de la película seleccionada por encima de la tabla -->
      <h2 v-if="selectedMovie" class="text-center my-4">
        Película seleccionada: {{ selectedMovie }}
      </h2>
  
      <!-- Mostrar la tabla solo cuando los datos ya se han cargado y no hay error -->
      <v-table v-if="!loading && !error" class="custom-table">
        <thead>
          <tr>
            <th class="highlight-header">Título</th>
            <th class="highlight-header">Calificación</th>
            <th class="highlight-header">IMDb</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="item in movies"
            :key="item.id"
            @click="selectMovie(item.movie)"
          >
            <td>{{ item.movie }}</td>
            <td>{{ item.rating }}</td>
            <td>
              <a :href="item.imdb_url" target="_blank">{{ item.imdb_url }}</a>
            </td>
          </tr>
        </tbody>
      </v-table>
  
      <!-- Mostrar <v-progress-linear> mientras los datos se cargan y no hay error -->
      <v-progress-linear
        v-if="loading && !error"
        indeterminate
        color="white"
        class="my-4"
      ></v-progress-linear>
    </v-container>
  </template>
  
  <script>
  import { onMounted, ref } from "vue";
  
  export default {
    setup() {
      const movies = ref([]);
      const loading = ref(true); // Variable de estado para la carga
      const error = ref(null); // Variable para almacenar el mensaje de error
      const selectedMovie = ref(null); // Variable para almacenar la película seleccionada
  
      const selectMovie = (movieName) => {
        selectedMovie.value = movieName;
      };
  
      onMounted(async () => {
        try {
          const response = await fetch("https://dummyapi.online/api/movies");
          if (!response.ok) {
            throw new Error("Error al obtener los datos del servidor");
          }
          const data = await response.json();
  
          // Ajusta la estructura si es necesario, aquí estoy suponiendo que 'data' es un array
          movies.value = data;
        } catch (err) {
          error.value = err.message; // Capturar y mostrar el error
        } finally {
          loading.value = false; // Indica que la carga ha terminado
        }
      });
  
      return {
        movies,
        loading,
        error,
        selectedMovie,
        selectMovie,
      };
    },
  };
  </script>
  
  <style scoped>
  /* Resalta los encabezados de la tabla */
  .custom-table .highlight-header {
    background-color: #4CAF50; /* Verde oscuro */
    color: white;
    text-align: left;
    padding: 10px;
    font-weight: bold;
    font-size: 16px;
    border-bottom: 2px solid #ffffff;
  }
  
  /* Estilo general para las filas */
  .custom-table tbody tr:hover {
    background-color: #353535; /* Cambiar color al pasar el mouse */
    cursor: pointer;
  }
  
  /* Opcional: estilizar celdas */
  .custom-table td {
    padding: 10px;
    border-bottom: 1px solid #ffffff;
  }
  </style>
  