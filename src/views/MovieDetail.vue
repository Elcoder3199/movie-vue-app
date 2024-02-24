<template>
  <div class="movieTemplete">
    <h1 class="title">{{ movie.Title }}</h1>
    <div class="loading" v-if="loading"></div>
    <transition name="slide-fade">
      <div class="container" v-if="exsist">
        <div class="image-box">
          <img :src="movie.Poster" alt="Image Poster" />
        </div>
        <div class="movie-detail">
          <div class="row" v-if="movie.Country || movie.Language">
            <h3>Language And Country</h3>
            <p>{{ movie.Country }} {{ movie.Language }}</p>
          </div>
          <div class="row" v-if="movie.Genre">
            <h3>Genre</h3>
            <p>{{ movie.Genre }}</p>
          </div>
          <div class="row" v-if="movie.Type">
            <h3>Type</h3>
            <p>{{ movie.Type }}</p>
          </div>
          <div class="row" v-if="movie.Year">
            <h3>Year</h3>
            <p>{{ movie.Year }}</p>
          </div>
          <div class="row" v-if="movie.Runtime">
            <h3>Runtime</h3>
            <p>{{ movie.Runtime }}</p>
          </div>
        </div>
      </div>
    </transition>

    <div class="plot" v-if="exsist">
      <h2>Plot</h2>
      <p>{{ movie.Plot }}</p>
    </div>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";
import { useRoute } from "vue-router";
import env from "@/env.js";
export default {
  name: "Movie-Detail",
  setup() {
    const movie = ref({});
    const route = useRoute();
    let loading = ref(false);
    let exsist = ref(false);
    onBeforeMount(() => {
      loading.value = true;
      fetch(
        `https://www.omdbapi.com/?apikey=${env.api_key}&i=${route.params.id}&plot=full`
      )
        .then((response) => response.json())
        .then((data) => {
          movie.value = data;
          exsist.value = true;
          loading.value = false;
          console.log(data);
        });
    });
    return {
      movie,
      exsist,
      loading,
    };
  },
};
</script>

<style scoped lang="scss">
.movieTemplete {
  .loading {
    top: 50%;
  }
  .title {
    text-align: center;
    margin: 30px 0;
    text-transform: uppercase;
    color: #fff;
    span {
      color: #c6bd0e;
    }
  }
  .container {
    padding: 0 40px;
    display: flex;
    justify-content: flex-start;
    width: 100%;
    align-items: center;
    grid-gap: 30px;
    @media (max-width: 1045px) {
      flex-direction: column;
    }
    .movie-detail {
      display: flex;
      flex-direction: column;
      justify-content: center;
      gap: 16px;
      text-align: center;
      flex-basis: 70%;
      @media (max-width: 760px) {
        width: 100%;
      }
      .row {
        display: flex;
        gap: 20px;
        border: solid 1px #444;
        align-items: center;
        min-height: 50px;
        h3 {
          color: #fff;
          line-height: 1.5;
          text-transform: uppercase;
          border-right: solid 1px #444;
          padding: 0 20px;
          @media (max-width: 760px) {
            font-size: 14px;
          }
        }
        p {
          color: #fff;
          font-weight: bold;
          font-size: 19px;
          word-spacing: 2px;
          @media (max-width: 760px) {
            font-size: 15px;
            line-height: 1.5;
            padding: 0 4px;
          }
        }
      }
    }
  }
  .plot {
    width: 99%;
    margin: 30px auto;
    h2 {
      color: #fff;
      padding: 10px;
      background-color: #444;
    }
    p {
      color: rgba(#fff, 0.4);
      padding: 10px;
      line-height: 1.5;
    }
  }
}
</style>
