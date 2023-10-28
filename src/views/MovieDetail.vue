<template>
  <section id="details" class="lg:scale-95  md:scale-90  sm:scale-[0.85] scale-[0.7] xs:scale-[0.55] vlg:transform w-full min-h-full flex flex-col justify-center items-center gap-4 ">
    <div v-if="movie" class=" w-[30rem] h-auto">
    <h1 class="text-lime-600 font-semibold tracking-widest text-3xl text-center my-6">{{ movie.title }}</h1>

    <img :src="'https://image.tmdb.org/t/p/w500' + movie.poster_path" alt="poster" class="self-center  w-full h-full rounded-lg object-cover">
    <p> <span class="italic text-slate-400 self-start mt-2">Release date: </span> {{ movie.release_date }}</p>
      <p> <span class="italic text-slate-400">Average vote: </span> {{ movie.vote_average }}</p>
    <p class=""><span class="italic text-slate-400">Description: </span>{{ movie.overview }}</p>
    <router-link to="/">
      <button  class="w-full text-center text-white bg-lime-600 rounded-md p-1 hover:bg-lime-700 mt-2" >
        GO HOME
      </button></router-link>
  </div>
  </section>

</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      movie: '',
      key:"416ceff41ddfa3d2dceb4594785e92c2"
    };
  },
  async created() {
    const movieId = this.$route.params.id;

    try {
      const response = await axios.get(`https://api.themoviedb.org/3/movie/${movieId}?api_key=${this.key}&language=en-US`);
      this.movie = response.data;
    } catch (err) {
      console.error('Error fetching :', err);
    }
  },
};
</script>
