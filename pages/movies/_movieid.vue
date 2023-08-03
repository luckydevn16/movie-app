<template>
    <LoadingPreview v-if="$fetchState.pending" />
    <div v-else class="container single-movie">
        <NuxtLink class="button" :to="{name:'index'}">Back</NuxtLink>
        <div class="movie-info">
            <div class="movie-img">
                <img 
                    :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" 
                    alt=""
                />
            </div>
            <div class="movie-content">
                <h1>{{movie.title}}</h1>
                <p class="movie-fact tagline">
                    <span>Tagline:</span> "{{movie.tagline}}"
                </p>
                <p class="movie-fact">
                    <span>Released:</span> 
                    {{
                        new Date(movie.release_date).toLocaleString('en-us', {
                            month: 'long',
                            day: 'numeric',
                            year: 'numeric',
                        })
                    }}
                </p>
                <p class="movie-fact">
                    <span>Duration:</span> {{movie.runtime}} min
                </p>
                <p class="movie-fact">
                    <span>Budget:</span> 
                    {{
                        movie.budget.toLocaleString('en-us', {
                            style: 'currency',
                            currency: 'USD',
                        })
                    }}
                </p>
                <p class="movie-fact">
                    <span>Revenue:</span> 
                    {{
                        movie.revenue.toLocaleString('en-us', {
                            style: 'currency',
                            currency: 'USD',
                        })
                    }}
                </p>
                <p class="movie-fact">
                    <span>Overview:</span> {{movie.overview}}
                </p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'SingleMovie',
    data() {
        return {
            movie: ''
        }
    },
    async fetch() {
        await this.getSingleMovie()
    },
    head() {
      return {
        title: this.movie.title,
      }
    },
    fetchDelay: 1000,
    methods: {
        async getSingleMovie() {
            const data = axios.get(
                `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=3890a375c54b66277129b4b3883a338c&language=en-US`
            )
            const result = await data;
            this.movie = result.data;
        }
    }
}
</script>

<style lang="scss">
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 2rem 1rem;
  .button {
    align-self: flex-start;
    margin-bottom: 2rem;
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;
    color: #fff;
      background-color: #222;
      padding: 2rem 1rem;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;
      }
    }
    .movie-content {
      h1 {
        font-size: 2rem;
        font-weight: 400;
        @media (min-width: 800px) {
          font-size: 3rem;
        }
      }
      .movie-fact {
        margin-top: 12px;
        font-size: .9rem;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: underline;
          color: #8c7ae6
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>