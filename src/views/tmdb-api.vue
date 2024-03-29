<template>
  <Search
    :valueSearch="valueSearch"
    v-on:keyup="keyup"
    v-on:search="search()"
    v-on:home="getMovies()"
  />

  <div class="flex flex-col items-center">
    <div class="flex flex-wrap items-start justify-center">
      <div v-for="movie in this.movies" v-bind:key="movie.id">
        <Card
          :title="movie.title"
          :description="movie.overview"
          :srcImage="`https://image.tmdb.org/t/p/original${movie.poster_path}`"
          :url="`/tmdb/movie/${movie.id}`"
          class="max-w-xs"
        ></Card>
      </div>
    </div>
    <Paginator
      :page="pagination.page"
      :totalPages="pagination.total"
      v-on:next="nextPage()"
      v-on:previous="previousPage()"
    />
  </div>
</template>

<script>
import Search from "../components/search/search.vue";
import { searchMovies, getPopularMovies } from "../services";
import Card from "../components/card/card.vue";
import Paginator from "../components/paginator/paginator.vue";
export default {
  components: { Search, Card, Paginator },

  mounted() {
    this.getMovies();
  },

  data() {
    return {
      valueSearch: "",
      movies: [],
      pagination: {
        page: 1,
        total: 0,
      },
    };
  },

  methods: {
    async getMovies() {
      const { success, data } = await getPopularMovies(this.pagination.page);

      if (!success) return;

      this.resetState();
      this.valueSearch = "";
      this.setMovies(data.results);

      this.pagination.page = data.page;
      this.pagination.total = data.total_pages;
    },

    async search() {
      const { success, data } = await searchMovies(
        this.valueSearch,
        this.pagination.page
      );

      if (!success) return;

      this.resetState();
      this.setMovies(data.results);

      this.pagination.page = data.page;
      this.pagination.total = data.total_pages;
    },

    setMovies(movies) {
      movies.forEach((movie) => {
        this.movies.push(movie);
      });
    },

    nextPage() {
      if (this.pagination.page < this.pagination.total) ++this.pagination.page;

      this.movies = [];
      this.valueSearch.length ? this.search() : this.getMovies();
    },

    previousPage() {
      if (this.pagination.page > 1) --this.pagination.page;
      this.movies = [];
      this.valueSearch.length ? this.search() : this.getMovies();
    },

    resetState() {
      this.movies = [];
      this.pagination = {
        page: 1,
        total: 0,
      };
    },

    keyup(valueSearch) {
      console.log("keyUP:", valueSearch);
      this.valueSearch = valueSearch;
    },
  },
};
</script>
