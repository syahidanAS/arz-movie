<template>
  <Loading v-if="$fetchState.pending" />
  <div v-else class="container single-movie">
    <nuxt-link class="button" :to="{ name: 'index' }">Back</nuxt-link>
    <div class="movie-info">
      <div class="movie-img">
        <img
          :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
          alt=""
        />
      </div>
      <div class="movie-content">
        <h1>Title: {{ movie.title }}</h1>
        <p class="movie-fact tagline">
          <span>Tagline:</span>"{{ movie.tagline }}"
        </p>
        <p class="movie-fact">
          <span>Released: </span>
          {{
            new Date(movie.release_date).toLocaleString('en-us', {
              month: 'long',
              day: 'numeric',
              year: 'numeric',
            })
          }}
        </p>
        <p class="movie-fact">
          <span>Duration:</span>{{ movie.runtime }} minutes
        </p>
        <p class="movie-fact">
          <span>Revenue:</span>
          {{
            movie.revenue.toLocaleString('en-us', {
              style: 'currency',
              currency: 'IDR',
            })
          }}
        </p>
        <p class="movie-fact"><span>Overview:</span>{{ movie.overview }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SingleMovie',
 
  data() {
    return {
      movie: '',
    }
  },
  async fetch() {
    await this.getSingleMovie()
  },
   head(){
	   return{
		   title: this.movie.title,
	   }
   },
  fetchDelay: 1000,
  methods: {
    async getSingleMovie() {
      try {
        const data = await this.$axios.$get(
          process.env.BASE_URL +
            `3/movie/${this.$route.params.movie}?api_key=${process.env.API_KEY}`
        )

        this.movie = data
      } catch (err) {
        alert(err + this.$route.params.movie)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
@import './_movie.scss';
</style>