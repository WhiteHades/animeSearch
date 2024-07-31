<script setup>
import Card from "./components/Card.vue";
import { ref } from "vue";

const search_query = ref("");
const animeList = ref([]);
document.title = "Anime Search";

const handleSearch = async () => {
  animeList.value = await fetch(
    `https://api.jikan.moe/v4/anime?q=${search_query.value}`
  )
    .then((res) => res.json())
    .then((json) => json.data);

  console.log(animeList.value);
};
</script>

<template>
  <div class="app">
    <header>
      <h1><strong>Anime</strong> Search</h1>
      <form class="search-box" @submit.prevent="handleSearch">
        <input
          type="search"
          class="search-field"
          placeholder="Search for an anime..."
          required
          v-model="search_query"
        />
      </form>
    </header>

    <main>
      <div class="cards">
        <Card v-for="anime in animeList" :key="anime.mal_id" :anime="anime" />
      </div>
    </main>
  </div>
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Fira Sans", sans-serif;
}

a {
  text-decoration: none;
}

header {
  padding-top: 50px;
  padding-bottom: 50px;

  h1 {
    color: #888;
    font-size: 42px;
    font-weight: 700;
    text-align: center;
    text-transform: uppercase;
    transition: 0.25s ease-in-out;
    padding-bottom: 30px;

    strong {
      color: #313131;
    }

    &:hover {
      color: #313131;
    }
  }

  .search-box {
    display: flex;
    justify-content: center;
    padding-right: 30px;
    padding-left: 30px;

    .search-field {
      appearance: none;
      background: none;
      border: none;
      outline: none;

      background-color: #f3f3f3;
      box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);

      display: block;
      width: 100%;
      max-width: 600px;
      padding: 15px;
      border-radius: 8px;

      color: #313131;
      font-size: 20px;

      transition: 0.25s;

      &::placeholder {
        color: #aaa;
      }

      &:focus,
      &:valid {
        color: #fff;
        background-color: #313131;
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
      }
    }
  }
}

main {
  max-width: 1200px;
  margin: 0 auto;
  padding-left: 30px;
  padding-right: 30px;

  .cards {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -8px;
  }
}
</style>
