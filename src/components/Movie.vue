<script setup>
import { ref } from 'vue';
import MovieModal from './MovieModal.vue';
import { onClickOutside } from '@vueuse/core';
const isModalOpen = ref(false);

const modal = ref(null);
onClickOutside(modal, () => (isModalOpen.value = false));

const props = defineProps({
  movie: {
    type: Object,
    required: true,
  },
});
</script>

<template>
  <div class="p-4 md:p-6 space-y-2 hover:opacity-95" ref="modal">
    <img
      :src="movie.show.image.medium"
      class="rounded-md w-full h-96 md:h-80 object-cover cursor-pointer"
      @click.native="isModalOpen = !isModalOpen"
      alt="movie image"
    />
    <div class="flex flex-row">
      <h1 class="text-lg font-semibold w-2/3">{{ movie.show.name }}</h1>
      <p
        class="text-md text-slate-500 ml-2 w-1/3 text-right"
        v-if="movie.show.rating.average !== null"
      >
        ⭐
        {{ movie.show.rating.average }}
      </p>
      <p class="text-md text-slate-500 ml-2 w-1/3 text-right" v-else>⭐ -</p>
    </div>
    <p class="text-sm text-slate-500">
      {{ movie.show.genres.join(', ') }}
    </p>

    <MovieModal :movie="movie" :isModalOpen="isModalOpen" />
  </div>
</template>
