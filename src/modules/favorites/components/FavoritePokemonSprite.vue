<template>
  <section class="pokemon-section">
    <div class="pokemon">
      <div class="info-container">
        <h1 :class="colorText()">
          {{ capitalize(name) }}
        </h1>
        <button
          :class="colorTextBackground()"
          class="cry-button"
          @click="loadCry()"
        >
          <i
            class="fa-solid fa-play"
          />Cry
        </button>
      </div>
      <div class="text-wrapper">
        <p class="sprite-text">
          Normal
        </p>
      </div>
      <div class="sprites-container">
        <figure class="pokemon-figure">
          <img
            class="pokemon-sprite"
            :src="loadSprite()"
            :alt="name"
          >
        </figure>
        <figure class="pokemon-figure">
          <img
            class="pokemon-sprite"
            :src="loadBackSprite()"
            :alt="name"
          >
        </figure>
        <figure class="pokemon-figure">
          <img
            class="pokemon-sprite"
            :src="loadShinySprite()"
            :alt="name"
          >
        </figure>
        <figure class="pokemon-figure">
          <img
            class="pokemon-sprite"
            :src="loadShinyBackSprite()"
            :alt="name"
          >
        </figure>
      </div>
      <div class="text-wrapper">
        <p class="sprite-text">
          Shiny
        </p>
      </div>
      <div class="main-card-button-container">
        <div class="card-change-wrapper tooltip-container">
          <button
            data-cy="remove-fav-btn"
            class="card-change"
            @click="authStore.deleteFavorite(id)"
          >
            <i class="fa-solid fa-trash" />
          </button>
          <p class="tooltiptext">
            {{ 'Remove ' + capitalize(name) + ' from favorites!' }}
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { ref } from 'vue'
import { useAuthStore } from '../store/authStore.js';
import { pokeapi } from '@/exports/pokeapi'
import { validPokemons } from '@/exports/pokemonValue';
export default {
    name: "FavoritePokemonSprite",
    props: {
        name: String,
        id: Number,
    },
    async setup(props) {
        const Pokemon = ref(null);
        const authStore = useAuthStore()
        const pokemonToFind = await fetch(`${pokeapi}/${props.name}`)
        const pokemon = await pokemonToFind.json()

        Pokemon.value = pokemon

        return {Pokemon, authStore}
    },
    // async mounted() {
    //     try {
    //         const pokemonToFind = await fetch(`${pokeapi}/${this.name}`)
    //         const pokemon = await pokemonToFind.json()
    //         this.Pokemon = pokemon

    //     } catch (error) {
    //         console.log(error)
    //     }
    // },
    data() {
        return {
            load: false
        };
    },
    methods: {
        loadSprite() {
            if (this.id > validPokemons) {               
                return new URL(`/assets/pokemon/` + this.id + `.png`, import.meta.url).href;
            }
            else {
                return new URL(`/assets/pokemon/versions/generation-v/black-white/animated/` + this.id + `.gif`, import.meta.url).href;
            }
        },
        loadCry() {
          const audio = new Audio("/assets/cries/" + this.id + ".wav");
                audio.volume = 0.3;
                audio.play();

        },
        loadShinySprite() {
            if (this.id > validPokemons) {
                return new URL(`/assets/pokemon/shiny/` + this.id + `.png`, import.meta.url).href;
            }
            else {
                return new URL(`/assets/pokemon/versions/generation-v/black-white/animated/shiny/` + this.id + `.gif`, import.meta.url).href;
            }
        },
        loadBackSprite() {
            if (this.id > validPokemons) {
                if (this.id > 697 || this.id < 701) {
                    return new URL(`/assets/pokemon/` + this.id + `.png`, import.meta.url).href;
                }
                else {
                    return new URL(`/assets/pokemon/versions/generation-v/black-white/back/` + this.id + `.png`, import.meta.url).href;
                }
            }
            else {
                return new URL(`/assets/pokemon/versions/generation-v/black-white/animated/back/` + this.id + `.gif`, import.meta.url).href;
            }
        },
        loadShinyBackSprite() {
            if (this.id > validPokemons) {
                if (this.id > 697 || this.id < 701) {
                    return new URL(`/assets/pokemon/shiny/` + this.id + `.png`, import.meta.url).href;
                }
                else {
                    return new URL(`/assets/pokemon/versions/generation-v/black-white/back/shiny/` + this.id + `.png`, import.meta.url).href;
                }
            }
            else {
                return new URL(`/assets/pokemon/versions/generation-v/black-white/animated/back/shiny/` + this.id + `.gif`, import.meta.url).href;
            }
        },
        capitalize(string) {
            return string.charAt(0).toUpperCase() + string.slice(1);
        },
        colorText() {
            return this.Pokemon.types[0].type.name;
        },
        colorTextBackground() {
            let background = this.Pokemon.types[0].type.name + "-b";
            return background;
        },
    }
}
</script>

<style scoped>
@import '@/assets/css/Pokemon.css';

.pokemon{
    width: 350px;
    height: 400px;
}

.card-change{
    margin-bottom: 18px;
    color: red;
}
</style>