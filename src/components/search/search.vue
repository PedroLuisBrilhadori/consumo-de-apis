<template>
  <div class="flex w-full px-10">
    <div
      v-if="!home"
      class="mr-5 bg-gradient-to-r from-orange-500 to-pink-500 rounded-lg px-5 flex justify-center items-center"
      @click="Home()"
    >
      <span class="material-symbols-outlined"> home </span>
    </div>

    <div class="w-full">
      <label
        for="default-search"
        class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-gray-300"
        >Search</label
      >
      <div class="relative">
        <div
          class="flex absolute inset-y-0 left-0 items-center pl-3 pointer-events-none"
        >
          <span class="material-symbols-outlined"> search </span>
        </div>

        <input
          type="search"
          id="default-search"
          class="block p-4 pl-10 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
          placeholder="Procure filmes, series..."
          required
          v-model="valueSearch"
          @keyup="this.$emit('keyup', valueSearch)"
        />

        <button
          @click="search()"
          class="text-white absolute right-2.5 bottom-2.5 bg-gradient-to-r from-orange-500 to-pink-500 hover:scale-105 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
        >
          Buscar
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  emits: ["search", "keyup", "home"],

  data() {
    return {
      home: true,
    };
  },

  props: {
    valueSearch: String,
  },

  methods: {
    search() {
      this.$emit("search");
      this.home = false;
    },

    Home() {
      this.home = true;
      this.$emit("home");
    },
  },
};
</script>
