<template>
  <div class="home">
    <app-genres></app-genres>
    <search></search>
    <router-link v-for="popular in populars"
                 :to="'/film/' + popular.id"
                 class="post">
      <film :item="popular" class="post__home"></film>
    </router-link>
    <app-pagination :actionName="itemName"></app-pagination>
  </div>
</template>

<script>
import Genres from '@/components/shared_components/Genres'
import Pagination from '@/components/shared_components/Pagination'
import Film from '@/components/shared_components/Film-component'
import Search from '@/components/shared_components/Search'

export default ({
  computed: {
    populars () {
      if (this.isFiltration) {
        return this.$store.state.FilmByGenres
      }
      return this.$store.state.resultsPopular
    },
    itemName () {
      if (this.isFiltration) {
        return 'getFilmsByGenre'
      }
      return 'getListPopular'
    },
    isFiltration () {
      return this.$store.state.FilmByGenres.length
    }
  },
  components: {
    appGenres: Genres,
    appPagination: Pagination,
    film: Film,
    search: Search
  },
  beforeMount () {
    this.$store.commit('startWaiter')
    this.$store.commit('setPage')
    this.$store.dispatch('getListPopular').then(() => {
      this.$store.commit('stopWaiter')
    })
  }
})
</script>

<style>
.post {
  margin: 0 15px 10px;
  padding: 15px 10px;
  background: #fff;
  border: 2px solid #222;
  border-radius: 10px;
}
.post__home {
  display: flex;
}
</style>
