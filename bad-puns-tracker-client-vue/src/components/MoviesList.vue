<template>
    <div>
        <span class="help is-info"  v-if="isLoading">Loading...</span>
        <table class="table" v-else>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Title</th>
                    <th>Bad Puns Count</th>
                    <th>Actions</th>
                </tr>
            </thead>
            <tbody>
                <template v-for="movie in movies">
                    <tr v-bind:key="movie.id">
                        <td>{{ movie.id }}</td>
                        <td>{{ movie.title }}</td>
                        <td>{{ movie.count }}</td>
                        <td>
                            <form @submit.prevent="onSubmit(movie)">
                                <button class="button is-primary" v-bind:class="{ 'is-loading' : isCountUpdating(movie.id) }">Increase Count</button>
                            </form>
                        </td>
                    </tr>
                </template>
            </tbody>
        </table>
        <movie-form @completed="addMovie"></movie-form>
    </div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'
import MovieForm from './MovieForm.vue'

export default {
    components: {
        MovieForm
    },
    data() {
        return {
            movies: {},
            isLoading: true,
            countUpdatingTable: []
        }
    },
    async created () {
        axios.defaults.headers.common['Authorization'] = `Bearer ${await this.$auth.getAccessToken()}`
        try {
            const response = await axios.get('http://localhost:8000/movies')
            this.movies = response.data
            this.isLoading = false
        } catch(e) {
            // handle authentication error here
        }
    },
    methods: {
        onSubmit(movie) {
            Vue.set(this.countUpdatingTable, movie.id, true)
            this.increaseCount(movie)
        },
        async increaseCount(movie) {
            axios.defaults.headers.common['Authorization'] = `Bearer ${await this.$auth.getAccessToken()}`
            axios.post('http://localhost:8000/movies/' + movie.id + '/count')
                .then(response => {
                    movie.count = response.data.count
                    this.countUpdatingTable[movie.id] = false
                })
                .catch(() => {
                    // handle authentication and validation errors here
                    this.countUpdatingTable[movie.id] = false
                })
        },
        isCountUpdating(id) {
            return this.countUpdatingTable[id]
        },
        addMovie(movie) {
            this.movies.push(movie)
        }
    }
}
</script>