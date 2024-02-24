<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt4154796">
        <img
          src="https://cdn.vox-cdn.com/thumbor/q1VhYtuVNtHtWRCb2icgjPzX3Sw=/0x0:1920x1005/fit-in/1200x630/cdn.vox-cdn.com/uploads/chorus_asset/file/15969338/surprise_marvel_releases_a_new_full_trailer_and_poster_for_avengers_endgame_social.jpg"
          alt="Feature Image"
          class="feature-img"
        />
        <div class="detalis">
          <h1>AVENGERS: ENDGAME</h1>
          <p>
            After the devastating events of Avengers: Infinity War (2018), the
            universe is in ruins due to the efforts of the Mad Titan, Thanos.
            With the help of remaining allies, the Avengers must assemble once
            more in order to undo Thanos's actions and undo the chaos to the
            universe, no matter what consequences may be in store, and no matter
            who they face...
          </p>
        </div>
      </router-link>
    </div>
    <form @submit.prevent="searchMovies()" class="search-box">
      <input
        type="text"
        placeholder="What Are Looking For..."
        class="inp-search"
        v-model="search"
        autofocus
      />
      <input type="submit" value="search" />
    </form>
    <div class="movie-list">
      <div class="loading" v-if="loading"></div>
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID">
          <transition name="slide-fade">
            <div class="product-image" v-if="exsist">
              <img :src="movie.Poster" alt="Movie Poster" />
              <div class="type">{{ movie.Type }}</div>
            </div>
          </transition>
          <div class="detail">
            <div class="y">{{ movie.Year }}</div>
            <div class="title">{{ movie.Title }}</div>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import env from "@/env.js";
export default {
  name: "HomeView",
  setup() {
    const search = ref("");
    const movies = ref([]);
    let loading = ref(false);
    let exsist = ref(false);
    const searchMovies = () => {
      if (search.value != "") {
        movies.value = "";
        loading.value = true;
        fetch(
          `https://www.omdbapi.com/?apikey=${env.api_key}&s=${search.value}`
        )
          .then((response) => response.json())
          .then((data) => {
            movies.value = data.Search;
            loading.value = false;
            exsist.value = true;
          });
      }
    };
    return {
      search,
      movies,
      loading,
      exsist,
      searchMovies,
    };
  },
};
</script>

<style scoped lang="scss">
%input {
  outline: none;
  border: none;
  background: none;
}
.home {

  .feature-card {
    position: relative;
    .feature-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;
      position: relative;
      z-index: -1;
    }
    .detalis {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(#000, 0.2);
      padding: 16px;
      h1 {
        color: #fff;
        margin-bottom: 10px;
      }
      p {
        color: #fff;
        line-height: 1.5;
        font-size: 15px;
      }
    }
  }
  .search-box {
    display: flex;
    align-items: center;
    margin: 14px 0;
    justify-content: center;
    @media (max-width: 380px) {
      
    }
    .inp-search {
      flex-basis: 70%;
      @extend %input;
      padding: 14px 23px;
      background-color: rgba(#000, 0.8);
      border-radius: 4px 0 0 4px;
      font-size: 16px;
      transition: 0.3s ease-in-out;
      color: rgba(#fff, 0.8);
      @media (max-width: 400px) {
      padding: 10px 18px;
      font-size: 14px;
    }
      &:hover {
        box-shadow: 0 0 8px rgba(#fff, 0.1);
      }
      &:focus {
        box-shadow: 0 0 8px rgba(#000, 0.2);
        background-color: rgba(#000, 0.7);
      }
    }
    input[type="submit"] {
      @extend %input;
      border-radius: 0 4px 4px 0;
      padding: 15px 13px;
      border: solid 1px #c6bd0e;
      color: #fff;
      font-weight: bold;
      cursor: pointer;
      text-transform: uppercase;
      @media (max-width: 400px) {
      padding: 10px 18px;
      font-size: 14px;
    }
      &:hover {
        background-color: #c6bd0e;
      }
    }
  }
  .movie-list {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 20px;
    align-items: center;
    justify-content: center;
    .movie {
      width: 100%;
      overflow: hidden;
      position: relative;
      &:hover {
        .detail {
          top: 76%;
          opacity: 1;
        }
      }
      position: relative;
      .product-image {
        position: relative;
        width: 100%;
        height: 400px;
        overflow: hidden;
        img {
          height: 100%;
          object-fit: cover;
          max-width: 100%;
          width: 100%;
        }
      }
      .type {
        position: absolute;
        left: 0;
        transition: 0.4s linear;
        top: 0;
        background-color: #c6bd0e;
        padding: 10px 23px;
        font-weight: bold;
        color: #fff;
      }
    }
    .detail {
      flex-direction: column;
      display: flex;
      justify-content: center;
      transition: 0.3s linear;
      background-color: #000;
      padding: 14px;
      text-transform: uppercase;
      gap: 10px;
      line-height: 1.5;
      height: 100px;
      opacity: 0;
      position: absolute;
      width: 100%;
      top: 100%;
      z-index: 99;
      .y {
        color: #fff;
        font-weight: bold;
      }
      .title {
        color: #fff;
        font-size: 15px;
      }
    }
  }
}
</style>
