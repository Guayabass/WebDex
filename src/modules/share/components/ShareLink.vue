<template>
  <h2 class="share-title">
    Share this Pokemon:
  </h2>
  <input
    v-if="dataReady"
    ref="pokemonLink"
    data-cy="share-txt"
    type="text"
    :value="'http://webdex.site/pokemon/' + Pokemon.name"
    readonly
    @focus="$event.target.select(), showText = true"
    @click="copy()"
    @blur="showText = false"
  >
  <p
    :class="showText ? 'enabled' : 'disabled'"
    class="copied-text"
  >
    Link copied to clipboard!
  </p>
</template>

<script>
import { pokeapi } from '@/exports/pokeapi'
export default {
    name: 'ShareLink',
    props: {
        // eslint-disable-next-line vue/require-default-prop
        route: String
    },
    data() {
        return {
            dataReady: false,
            Pokemon: {},
            showText: false,
        };
    },
    async mounted() {
        try {
            if (/^[a-zA-Z]+$/.test(this.route)) {
                const pokemonToFind = await fetch(`${pokeapi}/${this.route.toLowerCase()}`)
                const pokemon = await pokemonToFind.json()
                this.Pokemon = pokemon
                this.dataReady = true
                //console.log(this.route)
            } else {
                const pokemonToFind = await fetch(`${pokeapi}/${this.route}`)//aggara el pokemon con el id
                const pokemon = await pokemonToFind.json()
                this.Pokemon = pokemon
                this.dataReady = true
            }
        } catch (error) {
            console.log(error)
        }
    },
    methods: {
        copy() {
            this.$refs.pokemonLink.focus();
            document.execCommand('copy');
        },
        checkFocus() {
            if (this.showText) {
                this.showText = false
            } else {
                this.showText = true
            }
        }
    },
}

</script>

<style scoped>
@import url('https://fonts.cdnfonts.com/css/pokemon-solid');
@import url("https://fonts.googleapis.com/css2?family=Lato:wght@400;700&family=Roboto:wght@300;400;500;700&display=swap");
@import '@/assets/css/fadeTransition.css';

input {
    height: 50px;
    width: 50%;
    text-align: center;
    border: 2px solid dodgerblue;
    box-shadow: 0 0 8px 0 dodgerblue;
    font-family: 'Lato', sans-serif;
    border-radius: 4px;
    outline: none;
    color: #aaa;
    font-size: 24px;
    transition: all 300ms;
    cursor: pointer;
}

input:focus {
    border: 3px solid dodgerblue;
    box-shadow: 0 0 14px 0 dodgerblue;
}

input:hover {
    border: 3px solid dodgerblue;
    box-shadow: 0 0 14px 0 dodgerblue;
}

.share-title {
    font-family: 'Pokemon Solid', sans-serif;
    color: #207fb6;
}

.enabled {
    opacity: 1;
    transition: all 500ms ease;
}

.disabled {
    opacity: 0;
    transition: all 500ms ease;
}

.copied-text {
    font-family: 'Lato', sans-serif;
    margin-top: 8px;
    font-size: 16px;
    color: skyblue;
}

@media only screen and (max-width: 1000px) {
    input {
        width: 80%;
        font-size: 20px;
    }
}

@media only screen and (max-width: 768px) {
    input {
        width: 100%;
        font-size: 18px;
    }
}

@media only screen and (max-width: 420px) {
    input {
        width: 100%;
        font-size: 14px;
    }

}
</style>