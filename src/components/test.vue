<template>
    <div class="box">
        <div class="columns">
            <div v-for="m in movies" :key="m" class="column">   
                <figure class="image">
                    <img :src="imgSrc(m.id)">
                </figure>
            </div>
        </div>
    </div>
</template>

<script>

import { movies } from 'Others/movie.json'

export default {
    data() {
        return {
            movies
        }
    },
    methods: {
        imgSrc(movieId) {
            return `/movies/${movieId}.jpg`
        }
    }
}
</script>