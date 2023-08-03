<template>
  <div class="home">
    <HeaderSection />
    <div class="container search">
      <input 
        v-model.lazy="searchInput"
        type="text" 
        placeholder="Search" 
        @keyup.enter="$fetch"
      >
      <button v-show="searchInput !== ''" class="button" @click="clearSearch">Clear</button>
    </div>
    <LoadingPreview v-if="$fetchState.pending" />
    <div v-else class="container movies">
      <div v-if="searchInput !== ''" id="movie-grid" class="movies-grid">
        <div 
          v-for="(movie, index) in searchedMovies" 
          :key="index" 
          class="movie">
          <div class="movie-img">
            <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="">
            <p class="review">
              {{movie.vote_average}}
            </p>
            <p class="overview">
              {{movie.overview}}
            </p>
          </div>
          <div class="info">
            <p class="title">
              {{movie.title.slice(0,25)}}
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
            </p>
            <NuxtLink 
              class="button button-light" 
              :to="{name:'movies-movieid', params: {movieid: movie.id}}">
              More Info
            </NuxtLink>
          </div>
        </div>
      </div>
      <div v-else id="movie-grid" class="movies-grid">
        <div 
          v-for="(movie, index) in movies" 
          :key="index" 
          class="movie">
          <div class="movie-img">
            <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="">
            <p class="review">
              {{movie.vote_average}}
            </p>
            <p class="overview">
              {{movie.overview}}
            </p>
          </div>
          <div class="info">
            <p class="title">
              {{movie.title.slice(0,25)}}
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
            </p>
            <NuxtLink 
              class="button button-light" 
              :to="{name:'movies-movieid', params: {movieid: movie.id}}">
              More Info
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
    <div class="container pagination">
      <button class="button" @click="moreMovies">More Movies</button>
    </div>
    <AppFooter />
  </div>
</template>

<script>
import axios from 'axios';
import HeaderSection from "../components/HeaderSection.vue";
import AppFooter from "../components/AppFooter.vue";

export default {
    name: "IndexPage",
    components: { HeaderSection, AppFooter },
    data() {
      return {
        movies: [],
        searchInput: '',
        searchedMovies: [],
        page: 1
      }
    },
    async fetch() {
      if (this.searchInput === '') {
        await this.getMovies();
        return;
      }
      await this.searchMovies();
    },
    head() {
      return {
        title: 'Movie App - Latest Streaming',
        meta: [
          {
            hid: 'description',
            name: 'description',
            content: 'What to watch?'
          },
          {
            hid: 'keywords',
            name: 'keywords',
            content: 'movies, streaming, latest, stream, new, cinema'
          },
        ]
      }
    },
    fetchDelay: 1000,
    methods: {
      async getMovies() {
        const data = axios.get(
          `https://api.themoviedb.org/3/movie/now_playing?api_key=3890a375c54b66277129b4b3883a338c&language=en-US&page=${this.page}`
          );
        const result = await data;
        result.data.results.forEach(movie => {
          this.movies.push(movie);
        })
      },
      async searchMovies() {
        const data = axios.get(
          `https://api.themoviedb.org/3/search/movie?api_key=3890a375c54b66277129b4b3883a338c&language=en-US&page=1&query=${this.searchInput}`
        )
        const result = await data
        result.data.results.forEach((movie) => {
          this.searchedMovies.push(movie)
        })
      },
      clearSearch() {
        this.searchInput = ''
        this.searchedMovies = []
      },
      moreMovies() {
        this.page += 1;
        this.getMovies();
      }
    }
}
</script>

<style lang="scss">
.home {
  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }
  .search {
    display: flex;
    padding: 2rem 0 0 0;
    input {
      max-width: 16rem;
      width: 100%;
      padding: .5rem 1rem;
      font-size: 14px;
      border: none;
      &:focus {
        outline: none;
      }
    }
    .button {
      border-top-left-radius: 0;
      border-bottom-left-radius: 0;
    }
  }
  .movies {
    padding: 2rem 0;
    .movies-grid {
      display: grid;
      column-gap: 1rem;
      row-gap: 2rem;
      grid-template-columns: repeat(2, 1fr);
      @media (min-width: 500px) {
        grid-template-columns: repeat(3, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(4, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(5, 1fr);
      }
      .movie {
        position: relative;
        display: flex;
        flex-direction: column;
        .movie-img {
          position: relative;
          overflow: hidden;
          &:hover {
            .overview {
              transform: translateY(0);
            }
          }
          img {
            display: block;
            width: 100%;
            height: 100%;
          }
          .review {
            position: absolute;
            top: 0;
            left: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 40px;
            height: 40px;
            background-color: rgba(140,122,230,0.85);
            color: #fff;
            border-radius: 0 0 16px 0;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
          }
          .overview {
            line-height: 1.5;
            position: absolute;
            bottom: 0;
            background-color: rgba(140,122,230,0.85);
            padding: 1rem;
            color: #fff;
            transform: translateY(100%);
            transition: 0.3s ease-in-out all;
          }
        }
        .info {
          margin-top: auto;
          .title {
            margin-top: .5rem;
            color: #fff;
            font-size: 1.2rem;
          }
          .release {
            margin-top: .5rem;
            font-size: 0.9rem;
            color: #fff;
            opacity: .7;
          }
          .button {
            margin-top: 8px;
          }
        }
      }
    }
  }
  .pagination {
    padding-bottom: 1rem;
  }
}
</style>