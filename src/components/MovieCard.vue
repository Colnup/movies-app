
<template>
    <v-hover v-slot="{ isHovering, props }">
        <v-card :elevation="isHovering ? 10 : 2" color="white" v-bind="props" :class="{ 'on-hover': isHovering }">

            <v-img class="align-end" :src="config.url.photo_path + movie.backdrop_path" aspect-ratio="16/9"
                gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)" :class="{ 'hover_img': isHovering }">
                <v-card-title>{{ movie.title }}</v-card-title>
            </v-img>


            <v-card-text> {{ movie.overview ? movie.overview : 'Aucune description' }}</v-card-text>

            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="orange-lighten-2" variant="text" @click="showDetail">
                    Voir plus
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-hover>
</template>

<script>

import config from '@/config.json';

export default {
    name: 'MovieCard',
    props: {
        movie: {
            type: Object,
            default: () => { }
        }
    },
    data() {
        return {
            show: false,
            config
        }
    },
    methods: {
        showDetail() {
            // console.log('Emiting showDetail from MovieCard')
            this.$emit('showDetail', this.movie.id);
        }
    },
    emits: ['showDetail']
}


</script>

<style scoped>
.v-card {
    transition: box-shadow .4s ease-in-out;
}

.v-img {
    filter: brightness(75%);
    transition: filter .4s ease-in-out;
}

.hover_img {
    filter: brightness(100%);
}

.v-card-title {
    color: white;
}


.v-card-text {
    text-overflow: ellipsis;
    overflow: hidden;
    display: block;
    line-height: 2rem;
    white-space: nowrap;
}
</style>