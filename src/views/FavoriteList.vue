<template>
 <div class="container mx-auto px-4 pt-1">
   <ErrorMessage v-if="error"/>
   <Loader v-if="loader" />
    <h2 v-if="!favorite.length" class="uppercase tracking-wider text-yellow-500 text-lg font-semibold">There is nothing so far !</h2>

    <div class="popular-movies">
      <h2 class="uppercase tracking-wider text-orange-500 text-lg font-semibold">Favorite Movies</h2>
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-5 gap-8 ">
        <div
          class="mt-8"
          href="#"
          v-for="(film, index) in favorite"
          :key="index"
        >
          <router-link :to="{ name:'filmData', params:{ id:film.id }}">
            <img :src="'https://image.tmdb.org/t/p/w500/' + film.poster_path" alt="poster" class="hover:opacity-75 transition ease-in-out duration-150">
          </router-link>
          <div class="mt-2">
            <router-link :to="{ name:'filmData', params:{id: film.id }}" class="text-lg mt-2 hover:text-gray:300">
              {{ film.title }}
            </router-link>
            <div class="flex items-center text-gray-400 text-sm mt-1">
              <i class="fas fa-star"></i>
              <span class="ml-1">{{ film.vote_average * 10 + '%' }}</span>
              <span class="mx-2"> | </span>
              <span>{{ film.release_date }}</span>

              <FavoritesButton :iconSize="20"  :id="film.id" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import ErrorMessage from '@/components/errorMessage'
import Loader from '@/components/loader'
import FavoritesButton from '@/components/addToFavoritesButton'

export default {
  name: 'favoriteFilms',
  components: {
    ErrorMessage, Loader, FavoritesButton
  },
  data () {
    return {}
  },
  mounted () {
    this.$store.commit('favoriteMovies/loadIdsFromLocalStorage')
    this.$store.dispatch('favoriteMovies/getFavoriteMoviesIdsFromAPI')
  },
  computed: {
    ...mapState({
      favorite: state => state.favoriteMovies.favoriteMoviesDetails,
      error: state => state.error.isShowError,
      loader: state => state.loader.isShowLoader
    })

  }
}
</script>

<style>
</style>
