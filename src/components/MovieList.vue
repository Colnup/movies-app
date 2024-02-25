<template>
    <v-container>
        <v-row>
            <v-col v-for="movie in movieFiltered" :key="movie.id" md="4">
                <MovieCard :movie="movie" @showDetail="sendMovie" />
            </v-col>
        </v-row>

        <v-row>
            <v-col>
                <v-btn :loading="loadingMovies" @click="loadMovies">Charger plus de films</v-btn>
            </v-col>
        </v-row>

    </v-container>
</template>
<script>
import MovieCard from '@/components/MovieCard.vue';

import config from '@/config.json';
import axios from 'axios';
import lodash from 'lodash';


export default {
    name: 'MoviesList',
    components: { MovieCard },
    emits: ['showMovieDetailEmit'],
    data() {
        return {
            movies: [],
            movieListPage: 1,
            loadingMovies: false
        }
    },
    props: {
        currentSearch: {
            type: String,
            default: ''
        }
    },
    created() {
        this.loadMovies();
    },
    computed: {
        movieFiltered() {
            if (this.currentSearch === '') {
                return this.movies;
            }

            return lodash.filter(this.movies, movie => {
                return movie.title.toLowerCase().includes(this.currentSearch.toLowerCase());
            });
        }
    },
    methods: {
        loadMovies() {
            this.loadingMovies = true;
            const url = `${config.url.movie_list}&api_key=${config.api_key}&page=${this.movieListPage}`;
            axios
                .get(url)
                .then(response => {
                    this.movies.push(...response.data.results);
                    this.movieListPage++;
                    this.loadingMovies = false;
                })
        },
        sendMovie(movieId) {
            this.$emit('showMovieDetailEmit', movieId);
        }
    }
}

</script>