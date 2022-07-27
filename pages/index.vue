<template>
  <v-container>
    <SectionTitle title="Popular Movies" link="/movies" />
    <SectionMovies v-if="movies.length" :movies="movies" />
    <SectionTitle title="Upcoming Movies" link="/movies" />
    <SectionMovies v-if="upcoming.length" :movies="upcoming" />
  </v-container>
</template>

<script>
export default {
  name: "IndexPage",
  layout: "homepage",
  async asyncData({ $axios }) {
    try {
      const res = await $axios.$get("/movie/popular");
      const res2 = await $axios.$get("/movie/upcoming");
      return {
        movies: res.results.slice(0, 6),
        upcoming: res2.results.slice(0, 6),
      };
    } catch (e) {
      console.log(e);
    }
  },
};
</script>
