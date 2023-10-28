<template>
  <section id="details" class="w-full h-screen flex flex-col justify-center">
    <div v-if="movie">
    <h1 class="text-lime-600 font-semibold">{{ movie.title }}</h1>

    <img :src="'https://image.tmdb.org/t/p/w500' + movie.poster_path" alt="poster" class="object-cover self-center w-48 h-48">
    <p>Release date: {{ movie.release_date }}</p>
    <p>Average vote: {{ movie.vote_average }}</p>
    <p>{{ movie.overview }}</p>
    <router-link to="/" class="text-white bg-lime-600 rounded-md p-1 hover:bg-lime-700">Go Back</router-link>
  </div>
  </section>

</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      movie: null,
    };
  },
  async created() {
    const apiKey = '416ceff41ddfa3d2dceb4594785e92c2';
    const movieId = this.$route.params.id;

    try {
      const response = await axios.get(`https://api.themoviedb.org/3/movie/${movieId}?api_key=${apiKey}&language=en-US`);
      this.movie = response.data;
    } catch (error) {
      console.error('Error fetching movie details:', error);
    }
  },
};
</script>
