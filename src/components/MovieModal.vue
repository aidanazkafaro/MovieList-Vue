<script setup>
import { defineProps, Teleport, Transition, ref } from 'vue';
import { onClickOutside } from '@vueuse/core';

const movieSummary = '';
const modal = ref(null);
onClickOutside(modal, () => (isModalOpen.value = false));

const props = defineProps({
  movie: {
    type: Object,
    required: true,
  },
  isModalOpen: {
    type: Boolean,
    required: true,
  },
});
</script>

<template>
  <Teleport to="#modal">
    <Transition name="modal">
      <div
        class="fixed top-0 left-0 w-screen h-screen flex flex-wrap justify-center items-center bg-transparent"
        style="background-color: rgba(0, 0, 0, 0.5)"
        v-if="isModalOpen"
      >
        <div
          class="relative bg-white px-10 py-10 rounded-md shadow-md h-3/4 md:h-fit w-2/3 md:min-w-3xl md:max-w-3xl overflow-scroll md:overflow-hidden"
          ref="modal"
        >
          <button
            @click="isModalOpen = false"
            class="btn btn-blue border-2 absolute top-2 right-4 bg-none border-none cursor-pointer font-semibold"
          >
            x
          </button>

          <div class="flex flex-wrap md:flex-row">
            <img
              :src="movie.show.image.original"
              alt="movie image"
              class="w-[300px] h-auto"
            />
            <div class="w-full md:w-1/3 md:ml-10 flex flex-col">
              <div class="flex flex-row">
                <h1 class="text-lg font-semibold w-2/3">
                  {{ movie.show.name }}
                </h1>
                <p
                  class="text-md text-slate-500 ml-2 w-1/3 text-right"
                  v-if="movie.show.rating.average !== null"
                >
                  ⭐
                  {{ movie.show.rating.average }}
                </p>
                <p class="text-md text-slate-500 ml-2 w-1/3 text-right" v-else>
                  ⭐ -
                </p>
              </div>
              <p
                class="text-sm text-slate-950 mt-5 w-full"
                v-if="movie.show.summary !== null"
              >
                {{
                  (movieSummary = movie.show.summary.replace(
                    /<\/?[^>]+(>|$)/g,
                    ''
                  ))
                }}
              </p>
              <p class="text-sm text-slate-950 mt-5 w-56" v-else>No Summary</p>
              <p
                class="text-sm text-slate-500 mt-5 w-56"
                v-if="movie.show.language !== null"
              >
                Language:
                <span class="text-slate-950 font-semibold">{{
                  movie.show.language
                }}</span>
              </p>
              <p class="text-sm text-slate-500 mt-5 w-56" v-else>
                Language:
                <span class="text-slate-950 font-semibold"> - </span>
              </p>
              <p
                class="text-sm text-slate-500 mt-5 w-56"
                v-if="movie.show.network !== null"
              >
                Country:
                <span class="text-slate-950 font-semibold">{{
                  movie.show.network.country.code
                }}</span>
              </p>
              <p class="text-sm text-slate-500 mt-5 w-56" v-else>
                Country:
                <span class="text-slate-950 font-semibold"> - </span>
              </p>
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
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
