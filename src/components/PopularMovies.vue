<template>
  <div class="content w-ful min-h-full ">
  
    <header class="flex justify-between items-center md:p-8 p-4">
  
  
  <div class="logo  cursor-pointer">
   <h1 class="lg:text-4xl md:text-3xl font-bold text-2xl">Mo<span class="text-lime-700">Vies</span></h1>
  </div>
  
  <div class="input">
      <input type="text" placeholder="Search ..."  v-model="search"
      @keyup.enter="searchMovies()" class="sm:w-[14rem] focus:outline-lime-600 focus:shadow-lg focus:shadow-slate-500/50 pl-1 md:w-[17rem] lg:w-[22rem] py-[0.3rem] bg-gray-300 rounded-md focus:outline-none">
    </div>
  </header>
  
  
  
  
  
  
  <div>
    <h1 class="text-lime-600 font-semibold text-center tracking-widest text-3xl mt-24 ">Popular movies</h1>
    <section id="Movies" class="flex gap-2 flex-wrap justify-evenly ">
      <div v-for="movie in displayedMovies" :key="movie.id" class=" mt-12 hover:scale-105 transform  cursor-pointer delay-100 w-[18rem] flex flex-col gap-2 h-auto border border-slate-700 rounded-md p-2">
        <img :src="'https://image.tmdb.org/t/p/w500' + movie.poster_path" alt="poster" class="object-cover self-center w-full h-full">
  
        <h3 class="text-center font-semibold text-lime-600">{{ movie.title }}</h3>
        <p> <span class="italic text-slate-400">Release date: </span> {{ movie.release_date }}</p>
        <p> <span class="italic text-slate-400">Average vote: </span> {{ movie.vote_average }}</p>
        <router-link :to="'/movie/' + movie.id">
          <button class="text-white bg-lime-600 rounded-md p-1 hover:bg-lime-700 w-full">GO DETAILS</button>
  
        </router-link>
      </div>
    </section>
    <div class="text-center mt-6  pb-4">
  <button @click="prevPage" :disabled="currentPage === 1" class="cursor-pointer text-white bg-lime-600 rounded-md p-1 hover:bg-lime-700 w-24">Previous</button>
  <button @click="nextPage" :disabled="currentPage * itemsPage >= movies.length" :class="{ 'opacity-50': currentPage * itemsPage >= movies.length }" class="w-24 ml-2 text-white bg-lime-600 rounded-md p-1 hover:bg-lime-700">Next</button>
  </div>
  </div>  
  
  </div>
   
    </template>
    
    <script>
    import axios from 'axios';
    
    export default {
      data() {
        return {
          showMenu:true,
          movies: [],
          currentPage: 1,
          itemsPage: 8,
          search: "", 
          key:'416ceff41ddfa3d2dceb4594785e92c2'
  
        };
      },
      computed: {
        displayedMovies() {
          const start = (this.currentPage - 1) * this.itemsPage;
          const end = start + this.itemsPage;
          return this.movies.slice(start, end);
        },
      },
      mounted() {
        this.fetchMovies();
      },
      methods: {
        searchMovies() {
  
  const query=this.search.trim()
      if (query=== '') {
        this.fetchMovies();
        return;
      }
  
      axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=${this.key}&language=en-US&query=${query}&page=1`
      )
        .then((response) => {
          this.movies = response.data.results;
          this.currentPage = 1; 
        })
        .catch((err) => {
          console.error('Error fetching:', err);
        });
    },
        async fetchMovies() {
      
    
          try {
            const response = await axios.get(
              `https://api.themoviedb.org/3/movie/popular?api_key=${this.key}&language=en-US&page=1`
            );
  
            this.movies = response.data.results;
            console.log(this.movies);
          } catch (err) {
            console.error('Error fetching:', err);
          }
        },
        prevPage() {
          if (this.currentPage > 1) {
            this.currentPage--;
          }
        },
        nextPage() {
          if (this.currentPage * this.itemsPerPage < this.movies.length) {
            this.currentPage++;
          }
        },
      },
    };
    </script>
    <style>
  button:disabled{
    opacity:0.5;
  }</style>