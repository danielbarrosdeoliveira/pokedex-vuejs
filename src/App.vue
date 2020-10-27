<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img id="pokedex" src="./assets/pokemon.png" />
      <hr />
      <div class="columns">
        <input
          class="column is-half input"
          type="text"
          placeholder="Buscar pokemon pelo nome..."
          v-model="search"
        />
        <button
          id="btnSearch"
          class="column is-one-fifth button is-success"
          @click="searchPokemon"
        >
          Buscar
        </button>
      </div>

      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      search: "",
      url: "https://pokeapi.co/api/v2/pokemon?limit=151&offset=0",
    };
  },
  created: function () {
    axios.get(this.url).then((res) => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    });
  },
  components: {
    Pokemon,
  },
  methods: {
    searchPokemon: function () {
      this.filteredPokemons = this.pokemons;
      if (this.search == "" || this.search == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(
          (pokemon) => pokemon.name == this.search.toLowerCase()
        );
      }
    },
  },
  // computed: {
  //   resultSearch: function () {
  //     if (this.search == "" || this.search == " " || this.search.lenght === 0) {
  //       return this.pokemons;
  //     } else {
  //       return this.pokemons.filter((pokemon) => pokemon.name == this.search);
  //     }
  //   },
  // },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0 auto;
}
#app img#pokedex {
  display: block;
  max-width: 100%;
  width: 80%;
  padding: 1rem;
  margin: 0 auto;
}
#inputSearch {
  width: 50%;
}

div.columns {
  display: flex;
  align-items: center;
  justify-content: center;
  align-self: center;
}

button#btnSearch {
  display: flex;
  justify-content: center;
  margin-left: 0.2rem;
}
</style>
