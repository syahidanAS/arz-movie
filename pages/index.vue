<template>
  <div class="home">
    <!-- Hero section -->
    <Hero />

    <!-- Search section -->
    <div class="container search">
      <input
        v-model.lazy="searchInput"
        type="text"
        placeholder="Search movie"
        @keyup.enter="getSearchedMovie"
      />
      <button v-show="searchInput !== ''" class="button" @click="clearSearch">
        Clear Search
      </button>
    </div>

    <Loading v-if="$fetchState.pending" />

    <!-- Movies section -->
    <div v-else class="container movies">
      <div id="movie-grid" class="movies-grid">
        <div v-for="movie in movies" :key="movie.id" class="movie">
          <div class="movie-img">
            <img
              :src="`https://image.tmdb.org/t/p/w200${movie.poster_path}`"
              alt=""
            />
            <p class="review">
              {{ movie.vote_average }}
            </p>
            <p class="overview">
              {{ movie.overview }}
            </p>
          </div>
          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25) }}
              <span v-if="movie.title.length > 25">...</span>
            </p>

            <p class="release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
              <span v-if="movie.title.length > 25">...</span>
            </p>

            <NuxtLink
              class="button button-light"
              :to="{ name: 'movies-movie', params: { movie: movie.id } }"
              >Read More</NuxtLink
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchInput: '',
      isLoading: false,
    }
  },
  async fetch() {
    if (this.searchInput === '') {
      await this.getMovies()
      return
    }
    if (this.searchInput !== '') {
      await this.getSearchedMovie()
    }
  },

  head() {
    return {
      title: 'Movie App - Latest Streaming Movie Info',
      meta: [
        {
          head: 'description',
          name: 'description',
          content: 'Get all the latest streaming movies in theaters & online',
        },
        {
          head: 'keywords',
          name: 'keywords',
          content: 'movies, stream, stremaing',
        },
      ],
    }
  },

  fetchDelay: 3000,
  methods: {
    async getMovies() {
      try {
        const data = await this.$axios.$get(
          process.env.BASE_URL +
            `3/movie/now_playing?api_key=${process.env.API_KEY}`
        )

        this.movies = data.results
      } catch (err) {
        alert(err)
      }
    },
    async getSearchedMovie() {
      try {
        const data = await this.$axios.$get(
          process.env.BASE_URL +
            `3/search/movie?api_key=${process.env.API_KEY}&language=en-US&page=1&query=${this.searchInput}`
        )

        this.movies = data.results
      } catch (err) {
        alert(err)
      }
    },
    clearSearch() {
      this.searchInput = ''
      this.searchedMovies = []
    },
  },
}
</script>

<style lang="scss" scoped>
.loading {
  padding-top: 120px;
  align-items: flex-start;
}
@import './index.scss';
</style>