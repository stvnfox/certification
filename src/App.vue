<script lang="ts" setup>
import { reactive } from "vue";
import { movies } from "./helpers/movies.ts";

const moviesList = reactive(movies);

const setRating = (movie: any, rating: number) => {
  movie.rating = rating;
};
</script>

<template>
  <main
    class="min-w-screen min-h-screen flex flex-col bg-gray-900 justify-center items-center"
  >
    <section class="container mx-auto">
      <ul class="grid grid-cols-3 gap-6">
        <li
          v-for="movie in moviesList"
          :key="movie.id"
          class="flex flex-col bg-white rounded-md"
        >
          <img
            :src="movie.image"
            alt=""
            class="rounded-t-md h-[400px] object-cover w-full"
          />
          <div class="flex flex-col flex-grow p-4">
            <h2 class="text-xl font-semibold">{{ movie.name }}</h2>
            <ul class="flex items-center gap-1 mt-1 mb-2">
              <li
                v-for="(genre, index) in movie.genres"
                :key="`genre-${index}`"
                class="bg-purple-600 text-white rounded-2xl text-sm px-2 py-0.5"
              >
                {{ genre }}
              </li>
            </ul>
            <p class="flex-grow mb-6">{{ movie.description }}</p>
            <div class="flex">
              Rating: ({{ movie.rating }}/5)
              <ul class="flex items-center gap-1 ml-2">
                <li v-for="(star, ind) in 5" :key="`rating-${ind}`">
                  <button
                    :class="ind < movie.rating ? 'text-yellow-500' : 'text-gray-500'"
                    :disabled="movie.rating === ind + 1"
                    @click="setRating(movie, ind + 1)"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      fill="currentColor"
                      class="size-6"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M10.788 3.21c.448-1.077 1.976-1.077 2.424 0l2.082 5.006 5.404.434c1.164.093 1.636 1.545.749 2.305l-4.117 3.527 1.257 5.273c.271 1.136-.964 2.033-1.96 1.425L12 18.354 7.373 21.18c-.996.608-2.231-.29-1.96-1.425l1.257-5.273-4.117-3.527c-.887-.76-.415-2.212.749-2.305l5.404-.434 2.082-5.005Z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </button>
                </li>
              </ul>
            </div>
          </div>
        </li>
      </ul>
    </section>
  </main>
</template>
