<script lang="ts" setup>
import { ref, reactive } from "vue";
import { movies } from "./helpers/movies.ts";

const moviesList = ref(movies);

const setRating = (movieIndex: number, rating: number) => {
  moviesList.value[movieIndex].rating = rating;
};

const showAddForm = ref(false);
const toggleAddForm = () => {
  showAddForm.value = !showAddForm.value;

  resetFormState();
};

const form = reactive({
    name: "",
    description: "",
    image: "",
    genres: [],
    inTheatres: false,
    submitted: false,
});

const resetFormState = () => {
    form.name = "";
    form.description = "";
    form.image = "";
    form.genres = [];
    form.inTheatres = false;
    form.submitted = false;
};

const addMovie = () => {
    form.submitted = true;

    if (!form.name || !form.genres.length) {
        return;
    }

    moviesList.value.push({
        id: moviesList.value.length + 1,
        name: form.name,
        description: form.description,
        image: form.image,
        genres: form.genres,
        inTheatres: form.inTheatres,
    });

    toggleAddForm();
};
</script>

<template>
  <main
    class="min-w-screen min-h-screen flex flex-col bg-gray-900 justify-center items-center"
  >
    <section class="flex flex-col container mx-auto">
      <div class="self-end">
        <button
          class="self-end bg-purple-600 border-2 border-purple-600 hover:bg-white text-white hover:text-purple-600 rounded-lg transition-colors px-4 py-2 my-6"
          @click="toggleAddForm"
        >
          Add movie
        </button>
        <section
          v-if="showAddForm"
          class="fixed inset-0 bg-black/60 flex justify-center items-center z-10"
        >
          <div class="md:w-[600px] p-6 bg-gray-900 text-white rounded-lg">
            <h2 class="text-2xl font-semibold">Add movie</h2>
            <form @submit.prevent="addMovie">
              <fieldset>
                <label for="name" class="block mt-4 text-sm">Name</label>
                <input
                  type="text"
                  id="name"
                  v-model="form.name"
                  required
                  class="w-full bg-gray-800 text-white p-2 rounded-lg"
                />
                <p
                  v-if="!form.name && form.submitted"
                  class="text-red-500 text-sm"
                >
                  Name is required
                </p>
              </fieldset>
              <fieldset>
                <label for="description" class="block mt-4 text-sm"
                  >Description</label
                >
                <textarea
                  id="description"
                  v-model="form.description"
                  class="w-full bg-gray-800 text-white p-2 rounded-lg"
                ></textarea>
              </fieldset>
              <fieldset>
                <label for="image" class="block mt-4 text-sm">Image</label>
                <input
                  type="text"
                  id="image"
                  v-model="form.image"
                  class="w-full bg-gray-800 text-white p-2 rounded-lg"
                />
              </fieldset>
              <fieldset>
                <label for="genres" class="block mt-4 text-sm">Genres</label>
                <select
                  v-model="form.genres"
                  multiple
                  required
                  class="w-full bg-gray-800 text-white p-2 rounded-lg"
                >
                  <option>Drama</option>
                  <option>Action</option>
                  <option>Crime</option>
                  <option>Comedy</option>
                </select>
                <p
                  v-if="!form.genres.length && form.submitted"
                  class="text-red-500 text-sm"
                >
                  Genre is required
                </p>
              </fieldset>
              <fieldset class="flex items-center mt-3">
                <input
                  type="checkbox"
                  id="in-theatres"
                  v-model="form.inTheatres"
                  class="mr-2 w-5 h-5 accent-purple-600"
                />
                <label for="in-theatres">In theatres</label>
              </fieldset>
            </form>
            <div class="flex justify-between items-center mt-6">
              <button
                class="bg-red-500 border-2 border-red-500 hover:bg-white text-white hover:text-red-500 rounded-lg transition-colors px-4 py-1"
                @click="toggleAddForm"
              >
                Cancel
              </button>
              <button
                class="bg-purple-600 border-2 border-purple-600 hover:bg-white text-white hover:text-purple-600 rounded-lg transition-colors px-4 py-1"
                @click="addMovie"
              >
                Add movie
              </button>
            </div>
          </div>
        </section>
      </div>
      <ul class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
        <li
          v-for="(movie, movieIndex) in moviesList"
          :key="movie.id"
          class="relative flex flex-col bg-white rounded-md"
        >
          <img
            :src="movie.image"
            alt=""
            class="rounded-t-md h-[400px] object-cover w-full"
          />
          <div class="absolute right-2 top-2 text-yellow-500 h-12 w-12">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="currentColor"
              class="size-12"
            >
              <path
                fill-rule="evenodd"
                d="M10.788 3.21c.448-1.077 1.976-1.077 2.424 0l2.082 5.006 5.404.434c1.164.093 1.636 1.545.749 2.305l-4.117 3.527 1.257 5.273c.271 1.136-.964 2.033-1.96 1.425L12 18.354 7.373 21.18c-.996.608-2.231-.29-1.96-1.425l1.257-5.273-4.117-3.527c-.887-.76-.415-2.212.749-2.305l5.404-.434 2.082-5.005Z"
                clip-rule="evenodd"
              />
            </svg>
            <span
              class="text-gray-500 font-bold absolute left-1/2 top-1/2 -translate-y-1/2 -translate-x-1/2"
            >
              {{ movie.rating ?? "-" }}
            </span>
          </div>
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
              Rating: ({{ movie.rating ?? "0" }}/5)
              <ul class="flex items-center gap-1 ml-2">
                <li v-for="star in 5" :key="`rating-${star}`">
                  <button
                    :class="
                      star <= movie.rating ? 'text-yellow-500' : 'text-gray-500'
                    "
                    :disabled="movie.rating === star"
                    @click="setRating(movieIndex, star)"
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
