<template>
  <v-app>
    <Navbar />
    <Slider class="hidden-sm-and-down"/>
    <v-main>
      <Nuxt />
    </v-main>
    <Footer />
    <v-overlay :value="overlay" dark opacity="0.95">
      <Search />
      <v-btn v-if="overlay" @click="overlay = false">Close</v-btn>
    </v-overlay>
  </v-app>
</template>

<script>
import Search from "../components/Search.vue";
export default {
  name: "DefaultLayout",
  data() {
    return {
      overlay: false,
    };
  },
  created() {
    this.$nuxt.$on("openOverlay", ($event) => (this.overlay = $event));
  },
  watch: {
    $route(to, from) {
      this.overlay = false;
    },
  },
  components: { Search },
};
</script>
