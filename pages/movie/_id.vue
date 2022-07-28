<template>
  <v-container>
    <v-row>
      <v-col cols="12" sm="3">
        <LeftMovieInfo :data="data" />
      </v-col>
      <v-col cols="12" sm="9">
        <div class="d-md-flex align-center">
          <h2 class="display-1 font-weight-bold">{{ data.original_title }}</h2>
          <span class="display-1 ml-2 grey--text font-weight-light">
            {{ data.release_date.slice(0, 4) }}</span
          >
          <v-spacer></v-spacer>
          <Rating :data="data" />
        </div>
        <SocialShare />
        <v-divider class="mt-8"> </v-divider>
        <v-row class="mt-2">
          <v-col cols="12" sm="8">
            <h4 class="pink--text font-weight-bold title">Overview</h4>
            <p class="grey--text-darken">
              {{ data.overview }}
            </p>
            <!--IMAGES-->
            <h4 class="pink--text font-weight-bold title">Images</h4>
            <v-slide-group multiple show-arrows>
              <v-slide-item
                v-for="(image, index) in data.images.backdrops"
                :key="index"
                v-slot="{ toggle }"
              >
                <v-card @click="toggle" class="ma-4">
                  <v-img
                    cover
                    width="100"
                    height="160"
                    :src="`https://image.tmdb.org/t/p/w300${image.file_path}`"
                  ></v-img>
                </v-card>
              </v-slide-item>
            </v-slide-group>
            <!--CASTS-->
            <h4 class="pink--text font-weight-bold title">Casts</h4>
            <v-responsive>
              <v-virtual-scroll
                :height="300"
                item-height="48"
                :items="data.credits.cast"
              >
                <template v-slot:default="{ item }">
                  <v-list-item :key="item.id">
                    <v-list-item-avatar>
                      <v-img :src="getCastAvatar(item)"></v-img>
                    </v-list-item-avatar>
                    <v-list-item-title>
                      {{ item.name }}
                    </v-list-item-title>
                    <v-list-item-subtitle>
                      {{ item.character }}
                    </v-list-item-subtitle>
                  </v-list-item>
                </template>
              </v-virtual-scroll>
            </v-responsive>
          </v-col>
          <v-col cols="12" sm="4">
            <RightMovieInfo :data="data" />
          </v-col>
        </v-row>
        <!-- RECOMMENDATION -->
        <h4 class="pink--text font-weight-bold title mt-4">
          Movies Recommendation
        </h4>
        <v-row class="mt-2 mb-4">
          <v-col
            cols="12"
            sm="3"
            v-for="recommend in recommendations"
            :key="recommend.id"
          >
            <v-card :to="`/movie/${recommend.id}`" nuxt>
              <v-img
                :src="`https://image.tmdb.org/t/p/w300${recommend.poster_path}`"
              ></v-img>
            </v-card>
          </v-col>
        </v-row>
        <!-- REVIEWS -->
        <h4 class="pink--text font-weight-bold title mt-4">User Reviews</h4>
        <v-row
          class="mt-2 mb-4"
          v-for="review in reviews.results"
          :key="review.id"
        >
            <v-card class="mx-auto">
              <v-card-title class="d-md-flex align-center justify-space-between">
                <div>
                <v-list-item class="grow"
                  ><v-list-item-avatar color="grey darken-3">
                    <v-img
                      class="elevation-6"
                      alt=""
                      :src="`https://image.tmdb.org/t/p/w500${review.author_details.avatar_path}`"
                    ></v-img>
                  </v-list-item-avatar>

                  <v-list-item-content>
                    <v-list-item-title
                      class="black--text text-h6 font-weight-bold"
                      >{{ review.author }}</v-list-item-title
                    >
                  </v-list-item-content>
                </v-list-item>
                </div>
                <div >
                  <v-icon color="yellow">mdi-star</v-icon>
                  <span>{{review.author_details.rating}}/10</span>
                </div>
              </v-card-title>

              <v-card-text class="black--text">
                "{{ review.content }}"
              </v-card-text>

              <v-card-actions>
                <v-list-item class="grow">
                  <v-row align="center" justify="start">
                    <v-icon class="mr-1"> mdi-heart </v-icon>
                    <span class="subheading mr-2">256</span>
                    <span class="mr-1">·</span>
                    <v-icon class="mr-1"> mdi-share-variant </v-icon>
                    <span class="subheading">45</span>
                  </v-row>
                </v-list-item>
              </v-card-actions>
            </v-card>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  async asyncData({ params, $axios }) {
    try {
      const res = await $axios.$get(
        `/movie/${params.id}?append_to_response=credits,videos,images`
      );
      const res2 = await $axios.$get(`/movie/${params.id}/recommendations`);
      const res3 = await $axios.$get(`/movie/${params.id}/reviews`);
      return {
        data: res,
        recommendations: res2.results.slice(0, 4),
        reviews: res3,
      };
    } catch (e) {
      console.log(e);
    }
  },
  methods: {
    getCastAvatar(item) {
      if (item.profile_path) {
        return "https://image.tmdb.org/t/p/w45" + item.profile_path;
      }
      return "https://upload.wikimedia.org/wikipedia/commons/a/ac/Default_pfp.jpg";
    },
  },
};
</script>

<style></style>
