<script setup>
import axios from 'axios';
import Navbar from './components/Navbar.vue';
import { ref, onMounted } from 'vue';
import { onClickOutside } from '@vueuse/core';
import MovieModal from './components/MovieModal.vue';
import Movie from './components/Movie.vue';
const API_URL = 'http://api.tvmaze.com/search/shows?q=girls';

const movies = ref([]);

const modal = ref(null);

onClickOutside(modal, () => (isModalOpen.value = false));
const getMovies = async () => {
  try {
    const response = await axios.get(API_URL);
    movies.value = response.data;
  } catch (error) {
    console.error(error);
  }
};

onMounted(() => {
  getMovies();
});
</script>
<template>
  <Navbar />
  <div>
    <h1 class="w-full md:w-3/4 mx-auto text-2xl text-left mt-10 font-bold">
      Trending Now
    </h1>

    <div class="w-full md:w-3/4 justify-center mx-auto">
      <div class="flex flex-wrap">
        <div
          class="w-full md:w-1/2 xl:w-1/4 space-y-2 cursor-pointer"
          v-for="movie in movies"
          :key="movie.id"
        >
          <Movie :movie="movie" />
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.5s;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}
</style>
