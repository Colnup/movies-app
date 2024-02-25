<template>
    <div>
        <!-- <v-text-field v-model="search" @input="searchMovies" label="Rechercher un film" placeholder="Star Wars"
            prepend-inner-icon="mdi-magnify" clearable> -->

        <v-text-field v-model="search" label="Rechercher un film" placeholder="Top Gun" prepend-inner-icon="mdi-magnify"
            clearable>
        </v-text-field>
    </div>
</template>
<script>
export default {
    name: 'movieSearch',
    data() {
        return {
            search: ''
        }
    },
    emits: ['searchMovieEmit'],
    watch: {
        search() {
            this.searchMovies();
        }
    },
    methods: {
        searchMovies() {
            this.$emit('searchMovieEmit', this.search);
        }
    }
}
</script>