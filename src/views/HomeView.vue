<template>
  <MovieSearch @searchMovieEmit="searchMovie" />
  <MovieList @showMovieDetailEmit="showDetail" :currentSearch="currentSearch" />
  <v-dialog v-model="detailsOpen" width="800px">
    <MovieDetail :movieId="movieDetailId" @closeDetail="this.detailsOpen = false" />
  </v-dialog>
</template>

<script>
// import config from '@/config.json';
// import axios from 'axios';

import MovieList from '@/components/MovieList.vue';
import MovieDetail from '@/components/MovieDetail.vue';
import MovieSearch from '@/components/MovieSearch.vue';


export default {
  name: 'HomeView',
  components: { MovieList, MovieDetail, MovieSearch },
  data() {
    return {
      movieDetailId: null,
      detailsOpen: false,
      currentSearch: '',
    }
  },
  methods: {
    showDetail(movieId) {
      this.movieDetailId = movieId;
      this.detailsOpen = true;
      // console.log('showDetail', movieId);
      // console.log('this.detailsOpen', this.detailsOpen);
    },
    searchMovie(search) {
      // console.log("Setting search to", search);
      this.currentSearch = search;
    }
  }
}

</script>
