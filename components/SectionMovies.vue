<template>
  <v-row>
    <v-col cols="12" sm="4" v-for="movie in movies" :key="movie.id">
      <MovieCard :movie="movie" />
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: {
    movies: {
      type: Array,
    },
  },
};
</script>

<style></style>
