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
        class="fixed top-0 left-0 w-screen h-screen flex justify-center items-center bg-transparent"
        style="background-color: rgba(0, 0, 0, 0.5)"
        v-if="isModalOpen"
      >
        <div
          class="relative bg-white px-10 py-10 rounded-md shadow-md min-w-3xl max-w-3xl"
          ref="modal"
        >
          <button
            @click="isModalOpen = false"
            class="btn btn-blue border-2 absolute top-2 right-4 bg-none border-none cursor-pointer font-semibold"
          >
            x
          </button>

          <div class="flex flex-row">
            <img
              :src="movie.show.image.original"
              alt="movie image"
              class="w-[300px] h-auto"
            />
            <div class="w-2/3 ml-10 flex flex-col">
              <h1 class="font-bold text-3xl">
                {{ movie.show.name }}
              </h1>
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
