<template>
    <v-card color="white" :loading="!movie">

        <v-card-title v-if="!movie">
            Chargement...
        </v-card-title>

        <div v-if="movie">
            <v-img class="align-end" :src="config.url.photo_path + movie.backdrop_path"
                gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)">
                <v-card-title :class="{ 'white--text': true }">{{ movie.title }}</v-card-title>
            </v-img>

            <v-card-text>
                <v-row>
                    <v-col cols="12">
                        <v-row>
                            <v-chip v-for="genre in movie.genres" :key="genre.id" color="primary" label>{{ genre.name
                            }}</v-chip>
                        </v-row>
                    </v-col>

                    <v-col cols="12">
                        <v-chip color="primary" label>Date de sortie: {{ new Intl.DateTimeFormat('fr-FR',
                            {
                                dateStyle: 'long',
                                timeZone: 'CET'
                            }).format(new
                                Date(movie.release_date),) }}
                        </v-chip>
                    </v-col>

                    <v-col cols="12">
                        <v-rating :model-value="movie.vote_average / 2" color="yellow" readonly half-increments></v-rating>
                    </v-col>

                    <v-col cols="12">
                        <v-chip color="primary" label>Popularité: {{ movie.popularity }}</v-chip>
                    </v-col>

                    <v-col cols="12">
                        <v-chip color="primary" label>Langue originale: {{ new Intl.DisplayNames(['fr'], {
                            type: 'language'
                        }).of(movie.original_language) }}</v-chip>
                    </v-col>

                    <v-col cols="12">
                        <div class="synopsis">Synopsis: {{ movie.overview }}</div>
                    </v-col>
                </v-row>
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" text @click="closeDetail">Fermer</v-btn>
            </v-card-actions>
        </div>
    </v-card>
</template>


<script>
import config from '@/config.json';
import axios from 'axios';

export default {
    props: {
        movieId: {
            type: Number
        }
    },
    data() {
        return {
            movie: null,
            config: config,
            movieCache: {}
        };
    },
    watch: {
        movieId: {
            immediate: true,
            handler(newVal) {
                this.fetchMovieDetails(newVal);
            }
        },
    },
    methods: {
        fetchMovieDetails(movieId) {

            if (this.movieCache[movieId]) {
                this.movie = this.movieCache[movieId];
                return;
            }

            const url = `${config.url.movie_detail}/${movieId}?api_key=${config.api_key}&language=fr-FR`;
            axios.get(url)
                .then(response => {
                    this.movie = response.data;
                    this.movieCache[movieId] = this.movie;
                    // console.log(this.movie);
                })
                .catch(error => {
                    console.error('Error fetching movie details:', error);
                });
        },
        closeDetail() {
            this.$emit('closeDetail');
        }

    }
};

</script>

<style scoped>
.synopsis {
    white-space: pre-wrap;
}

.white--text {
    color: white;
}
</style>