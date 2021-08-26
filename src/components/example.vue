<template>
  <div id="app">
    <img class="logo" src="./assets/pokemon-logo.jpg">
    <h1>PokeGuía</h1>
    <div>
      <label>Nombre:</label>
      <input v-model="pokemon.name" v-on:keyup.enter="buscar" />
      <button @click="buscar">Buscar</button>
    </div>
    <div>
      <img :src="pokeFoto" />
    </div>
    <div>
      <h2>Movimientos</h2>
      <ol class="lista">
        <li v-for="(movimiento, index) in movimientos"
        :key="index"
        v-text="movimiento.move.name"></li>
      </ol>
    </div>
    <div>
      <h2>Habilidades</h2>
      <ol class="lista">
        <li v-for="(habilidad, index) in habilidades"
        :key="index"
        v-text="habilidad.ability.name"></li>
      </ol>
    </div>
  </div>
</template>
<script>
import HelloWorld from './components/HelloWorld.vue'
export default {
  data() {
    return {
      pokemon: {
        name: 'pikachu',
        sprites: {
          front_default: ''
        },
        moves: [],
        abilities: []
      },
      actualizacion: ''
    }
  },
  created() {
    this.buscar()
  },
  methods: {
    buscar(){
      fetch(`https://pokeapi.co/api/v2/pokemon/${this.pokemon.name}`)
      .then(response => response.json())
      .then(json => this.pokemon = json)
    }
  },
  computed: {
    pokeFoto(){
      return this.pokemon.sprites.front_default
    },
    movimientos(){
      return this.pokemon.moves
    },
    habilidades(){
      return this.pokemon.abilities
    }
  },
}
</script>